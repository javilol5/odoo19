# Manual de Implantación de Odoo 19 con Docker Compose

Este repositorio contiene la configuración, documentación y evidencia paso a paso de la implantación y uso de **Odoo 19 Community**, **PostgreSQL** y **pgAdmin 4** desplegados mediante Docker Compose, cumpliendo con los requisitos establecidos en la guía de la actividad.

---

## 📑 ÍNDICE

> 📌 **Navegación Rápida:**
>
> * 🗄️ [1. Bases de Datos](#1-bases-de-datos)
> * 🏷️ [2. Ejercicio 1 · Tipos de Producto](#2-ejercicio-1--tipos-de-producto)
> * 🔄 [3. Ejercicio 2 · Recorrido Completo](#3-ejercicio-2--recorrido-completo)
> * 🔍 [4. Ejercicio 3 · Búsqueda Guardada](#4-ejercicio-3--búsqueda-guardada)
> * 🛠️ [5. Ejercicio 4 · Modo Desarrollador](#5-ejercicio-4--modo-desarrollador)
> * 🚨 [6. Incidencias](#6-incidencias)
> * 🤖 [7. Declaración de Uso de IA](#7-declaración-de-uso-de-ia)

---

## 1. Bases de Datos

### Despliegue del Entorno
Se levantan los servicios de Odoo, base de datos PostgreSQL y pgAdmin ejecutando `docker compose up -d` en el directorio del proyecto.

![1.png](images/1.png)
*Imagen 1: Inicio de los contenedores en Docker Compose.*

### Configuración y Creación en el Gestor de Bases de Datos
Accediendo a `http://localhost:8069/web/database/manager`, se gestionan las bases de datos del sistema (`demo` y `primo` / bases de trabajo), configurando la localización en **Idioma: Spanish / Español** y **País: Spain** (España) para habilitar la localización fiscal española (plan contable e impuestos).

![101.png](images/101.png)
*Imagen 101: Creación de la base de datos en el gestor con idioma Español, país España y datos de demostración.*


---

## 2. Ejercicio 1 · Tipos de Producto

Se han revisado y configurado productos representativos de cada uno de los tipos disponibles en Odoo 19, verificando sus impuestos asignados:

### A. Bienes (Físicos con y sin rastreo de inventario)
* **Alfombrilla de escritorio (`FURN_0002`):** Producto de tipo *Bienes* con rastreo de inventario por cantidad. Precio de venta: $1,98. **Impuestos asignados:** Sin impuestos de venta/compra asignados por defecto.
* **Mesa de comedor para exteriores:** Producto de tipo *Bienes* sin rastreo de inventario activo. Precio de venta: $589,00. **Impuestos asignados:** Ninguno.

![102.png](images/102.png)
*Imagen 102: Ficha del producto tipo Bienes "Alfombrilla de escritorio" (sin impuestos).*

![105.png](images/105.png)
*Imagen 105: Ficha del producto tipo Bienes "Mesa de comedor para exteriores" (sin impuestos).*

### B. Servicios
* **Alojamiento de hotel:** Producto de tipo *Servicio* con facturación sobre cantidades entregadas. Precio: $400,00. **Impuestos asignados:** Ninguno.
* **Hora Soporte:** Producto de tipo *Servicio* creado para la actividad. Precio: $40,00. **Impuestos asignados:** IVA de venta del `21% B` e IVA de compra del `21% B`.

![103.png](images/103.png)
*Imagen 103: Ficha del producto tipo Servicio "Alojamiento de hotel" (sin impuestos).*

![109.png](images/109.png)
*Imagen 109: Ficha del producto tipo Servicio "Hora Soporte" con impuesto del 21% asignado.*

### C. Combinación
* **Set para oficina:** Producto de tipo *Combinación* compuesto por opciones de escritorios, sillas y accesorios. Precio: $160,00. **Impuestos asignados:** Ninguno en la ficha principal.

![104.png](images/104.png)
*Imagen 104: Ficha del producto tipo Combinación "Set para oficina".*

---

## 3. Ejercicio 2 · Recorrido Completo

A continuación se detalla el flujo completo desde la creación del cliente y los productos con su IVA correspondiente hasta la validación y pago de la factura:

### Paso 1: Creación del Cliente
Se da de alta el contacto individual **NoSergio NoFernandez NoFernandez**, con dirección en Pontevedra (España), puesto *Sergiador*, NIF válido `ESA12345674` y la etiqueta `NoSergio`.

![108.png](images/108.png)
*Imagen 108: Ficha del cliente "NoSergio NoFernandez NoFernandez" guardado correctamente.*

### Paso 2: Preparación de Productos con Impuestos (IVA)
Se configuran los dos productos que formarán parte de la venta con sus respectivos tipos impositivos en España:
* **Hora Soporte** (Servicio): $40,00 + **IVA 21% B** ($48,40 IVA incluido).
* **Libro Tecnico** (Bienes): $25,00 + **IVA 4% B** ($26,00 IVA incluido).

![110.png](images/110.png)
*Imagen 110: Ficha del producto "Libro Tecnico" con el IVA superreducido del 4% B asignado.*

### Paso 3: Creación del Presupuesto (`S00025`)
Se genera el presupuesto `S00025` para el cliente añadiendo una unidad de cada producto. En las líneas se aprecia claramente el impuesto aplicado a cada línea (`21% B` y `4% B`), sumando una base imponible de **65,00 €**, IVA 4% (**1,00 €**), IVA 21% (**8,40 €**) y un Total de **74,40 €**.

![111.png](images/111.png)
*Imagen 111: Presupuesto S00025 con el desglose de líneas e impuestos visibles.*

### Paso 4: Confirmación de Pedido y Generación de Borrador de Factura
Tras resolver el aviso del almacén y confirmar el pedido de venta, se genera el borrador de factura asociado al pedido `S00025` manteniendo intactos los importes e impuestos por línea.

![113.png](images/113.png)
*Imagen 113: Borrador de factura generado a partir del pedido S00025.*

### Paso 5: Validación, Documento PDF y Pago de la Factura (`INV/2026/00010`)
Se confirma la factura quedando registrada como `INV/2026/00010`, se genera el documento PDF oficial y se registra el cobro, pasando al estado **PAGADO** con importe adeudado de **0,00 €**.

![114.png](images/114.png)
*Imagen 114: Factura INV/2026/00010 exportada en PDF con el desglose fiscal completo.*

![115.png](images/115.png)
*Imagen 115: Factura registrada y marcada con la banda verde de PAGADO.*

![116.png](images/116.png)
*Imagen 116: Registro de auditoría (chatter) confirmando la validación y el pago de la factura.*

---

## 4. Ejercicio 3 · Búsqueda Guardada

En el módulo de **Facturación → Clientes → Facturas**, se aplica un filtro personalizado para mostrar únicamente las facturas de cliente en estado **Registrado**, se agrupan los resultados por **Cliente** en vista de lista y se guarda la búsqueda en **Favoritos** bajo el nombre **`FacturasUwU`**.

![117.png](images/117.png)
*Imagen 117: Lista de facturas publicadas agrupadas por cliente con el favorito "FacturasUwU" activo.*

![118.png](images/118.png)
*Imagen 118: Configuración técnica y dominio del filtro favorito guardado "FacturasUwU".*

---

## 5. Ejercicio 4 · Modo Desarrollador

### Vías de Activación
El modo desarrollador en Odoo 19 se puede activar mediante dos métodos:
1. **Desde la interfaz gráfica:** Accediendo a *Ajustes → Herramientas de desarrollador → Activar el modo desarrollador* (hace visible el icono del insecto/bug en la barra superior).
2. **Mediante la URL:** Añadiendo el parámetro `?debug=1` directamente en la barra de direcciones del navegador (ej. `http://localhost:8069/odoo/contacts/55?debug=1`).

![119.png](images/119.png)
*Imagen 119: Modo desarrollador activado (`?debug=1` en la URL e icono de depuración en la barra superior).*

### Inspección de Campos y Tabla de Nombres Técnicos
Al situar el cursor sobre los distintos elementos de las vistas de Contactos y Pedidos de Venta, Odoo despliega la información técnica de cada campo y modelo:

![120.png](images/120.png)
*Imagen 120: Inspección del campo "Puesto de trabajo" (`function`) en el modelo `res.partner`.*

![121.png](images/121.png)
*Imagen 121: Inspección del campo "NIF" (`vat`) en el modelo `res.partner`.*

![122.png](images/122.png)
*Imagen 122: Inspección del campo "Etiquetas" (`category_id`) en el modelo `res.partner`.*

![123.png](images/123.png)
*Imagen 123: Inspección del campo "Cliente" (`partner_id`) en el modelo `sale.order`.*

![124.png](images/124.png)
*Imagen 124: Inspección del campo "Fecha del pedido" (`date_order`) en el modelo `sale.order`.*

| Vista / Formulario | Etiqueta Visible | Modelo | Nombre Técnico | Tipo de Campo |
| :--- | :--- | :--- | :--- | :--- |
| Formulario de Contacto | Compañía relacionada | `res.partner` | `parent_id` | `many2one` |
| Formulario de Contacto | Puesto de trabajo | `res.partner` | `function` | `char` |
| Formulario de Contacto | NIF | `res.partner` | `vat` | `char` |
| Formulario de Contacto | Etiquetas | `res.partner` | `category_id` | `many2many` |
| Pedido de Venta | Cliente | `sale.order` | `partner_id` | `many2one` |
| Pedido de Venta | Fecha del pedido | `sale.order` | `date_order` | `datetime` |

---

## 6. Incidencias

| Fecha | Síntoma / Problema | Causa | Solución |
| :--- | :--- | :--- | :--- |
| 10/11/2026 | Los cambios en los archivos de configuración no se reflejaban al reiniciar los contenedores. | Los archivos en VS Code no tenían el autoguardado activado y no se habían guardado manualmente. | Se guardaron todos los archivos manualmente antes de ejecutar `docker compose up -d`. |
| 10/11/2026 | Error de autenticación al intentar acceder a pgAdmin 4. | Se introdujo una contraseña incorrecta respecto a la definida en el archivo `.env` / `docker-compose.yml`. | Se verificó e introdujo la contraseña correcta configurada para el servicio pgAdmin. |
| 10/11/2026 | Al crear productos, se aplicaba un impuesto del 15% por defecto en lugar del IVA español (21% / 4%). | La base de datos inicial se creó sin seleccionar España como país, cargando la localización genérica. | Se creó una nueva base de datos seleccionando **País: Spain** e **Idioma: Español** para cargar el plan fiscal español. |
| 18/11/2026 | Error de validación al guardar el NIF del cliente (`12345678F`). | El módulo de localización española valida el formato oficial y el dígito de control del NIF/CIF. | Se ajustó el identificador a un formato de NIF/CIF español válido (`ESA12345674`). |
| 18/11/2026 | Advertencia *"Cree un almacén para la compañía ES Company"* al intentar confirmar el pedido `S00025` (Imagen 112). | La compañía recién configurada no tenía un almacén asociado para gestionar la entrega del producto físico (*Libro Tecnico*). | Se accedió a la configuración de inventario/almacenes para asignar el almacén a la compañía antes de confirmar el pedido. |

![112.png](images/112.png)
*Imagen 112: Advertencia de Odoo por falta de almacén asignado a la compañía al confirmar el pedido.*

---

## 7. Declaración de Uso de IA

* **Herramienta utilizada:** Gemini (Google).
* **Para qué se utilizó:**
  1. Para diagnosticar por qué no se aplicaban los cambios en el entorno Docker (descubriendo posteriormente por cuenta propia que el problema se debía a no guardar los archivos en VS Code).
  2. Para estructurar, redactar y completar el archivo `ReadmeNuevo.md`, organizando las capturas de pantalla (`101.png` a `124.png`), extrayendo los metadatos técnicos de las capturas en modo desarrollador y documentando las incidencias encontradas.
* **Qué se cambió de su salida:** Se revisaron y ajustaron manualmente las descripciones de los pasos realizados, se verificaron los nombres exactos de las capturas asociadas a cada ejercicio y se completaron los detalles específicos de las incidencias reales ocurridas durante la práctica (como el ajuste del NIF español y la creación del almacén).