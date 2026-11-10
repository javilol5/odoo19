# Manual de Implantación de Odoo 19 con Docker Compose

Este repositorio contiene la configuración y la guía paso a paso para la implantación de un entorno de trabajo con **Odoo 19 Community**, **PostgreSQL** y **pgAdmin 4** en Docker, siguiendo los requerimientos de la actividad.

---

## 📑 ÍNDICE

> 📌 **Navegación Rápida:**
>
> * 🗄️ [1. Bases de Datos y Gestión del Entorno](#1-bases-de-datos-y-gestión-del-entorno)
> * 🧩 [2. Aplicaciones y Módulos](#2-aplicaciones-y-módulos)
> * 🏷️ [3. Ejercicio 1: Tipos de Productos e Impuestos](#3-ejercicio-1-tipos-de-productos-e-impuestos)
> * 🔄 [4. Ejercicio 2: Recorrido Completo (Del Cliente a la Factura Pagada)](#4-ejercicio-2-recorrido-completo-del-cliente-a-la-factura-pagada)
> * 🧾 [5. Facturación de Proveedores y Registros Adicionales](#5-facturación-de-proveedores-y-registros-adicionales)
> * 🔍 [6. Ejercicio 3: Búsquedas Guardadas y Filtros](#6-ejercicio-3-búsquedas-guardadas-y-filtros)
> * 🛠️ [7. Ejercicio 4: Modo Desarrollador](#7-ejercicio-4-modo-desarrollador)
> * 🚨 [8. Incidencias y Solución de Problemas](#8-incidencias-y-solución-de-problemas)
> * 🤖 [9. Declaración del Uso de Inteligencia Artificial](#9-declaración-del-uso-de-inteligencia-artificial)

---

## 1. Bases de Datos y Gestión del Entorno

### Despliegue de los Contenedores
Para iniciar la infraestructura se ejecuta el siguiente comando en la carpeta raíz del proyecto (`~/Documentos/odoo19`):

```bash
docker compose up -d
```

![1.png](images/1.png)
*Imagen 1: Descarga e inicio de los servicios de Odoo, PostgreSQL y pgAdmin mediante Docker Compose.*

---

### Creación de la Base de Datos en Odoo
Accediendo a `http://localhost:8069`, se crea la base de datos rellenando los datos solicitados (Master Password, nombre de BD, correo, contraseña, idioma *Español* y país *España*).

![2.png](images/2.png)
*Imagen 2: Formulario de creación de la base de datos "CASAL".*

![4.png](images/4.png)
*Imagen 4: Gestor de bases de datos de Odoo mostrando la base de datos "CASAL" ya registrada.*

---

### Administración con pgAdmin
Se accede a `http://localhost:5050` para conectarse al gestor de bases de datos PostgreSQL mediante pgAdmin.

![5.png](images/5.png)
*Imagen 5: Pantalla de login en pgAdmin.*

---

## 2. Aplicaciones y Módulos

Dentro del panel principal de Odoo, se instalan y gestionan las aplicaciones requeridas (**Contactos**, **Ventas**, **Facturación** e **Inventario**).

![3.png](images/3.png)
*Imagen 3: Panel de Aplicaciones de Odoo mostrando la búsqueda de módulos.*

---

## 3. Ejercicio 1: Tipos de Productos e Impuestos

Se han configurado productos representativos para cada uno de los tipos admitidos por Odoo 19:

### A. Bienes con Rastrear Inventario (Físicos con Control de Stock)
* **Silla de conferencias:** Producto físico con control de existencias. Precio de venta: $33,00.
* **Cajón (`FURN_8855`):** Producto físico con control de inventario por lotes. Precio de venta: $110,50.

![6.png](images/6.png)
*Imagen 6: Ficha del producto "Silla de conferencias".*

![8.png](images/8.png)
*Imagen 8: Ficha del producto "Cajón" con rastreo por lotes.*

---

### B. Servicios y Bienes sin Rastreo
* **Alojamiento de hotel:** Producto de tipo Servicio con política de facturación basada en entregas. Precio: $400,00.
* **Hora de soporte:** Producto de tipo Servicio con IVA de venta del 21%. Precio: $40,00.
* **Libro Tecnico:** Bien de tipo consumible/sin rastreo con IVA reducido del 4%. Precio: $25,00.

![7.png](images/7.png)
*Imagen 7: Ficha del producto "Alojamiento de hotel".*

![13.png](images/13.png)
*Imagen 13: Ficha del producto "Hora de soporte" (Impuesto de venta al 21%).*

![14.png](images/14.png)
*Imagen 14: Ficha del producto "Libro Tecnico" (Impuesto de venta al 4%).*

---

### C. Combinación
* **Set para oficina:** Producto compuesto por opciones combinadas de escritorios, sillas y accesorios. Precio: $160,00.

![9.png](images/9.png)
*Imagen 9: Ficha del producto tipo Combinación "Set para oficina".*

---

## 4. Ejercicio 2: Recorrido Completo (Del Cliente a la Factura Pagada)

### 1. Gestión de Contactos
Se gestionan contactos de tipo Compañía e Individuo, vinculando los contactos individuales a su empresa correspondiente.

![10.png](images/10.png)
*Imagen 10: Ficha de la compañía Acme Corporation con sus empleados asociados.*

![11.png](images/11.png)
*Imagen 11: Ficha individual del contacto Addison Olson dentro de Acme Corporation.*

Se crea un nuevo cliente individual denominado **Sergio Fernandez Rodriguez** con NIF `12345677J`, correo y etiqueta **VIP**.

![12.png](images/12.png)
*Imagen 12: Creación del nuevo cliente Sergio Fernandez Rodriguez.*

---

### 2. Creación y Conversión de Presupuesto a Pedido
Se genera un presupuesto para el cliente Sergio Fernandez Rodriguez agregando los productos:
* 1x **Libro Tecnico** ($25,00, IVA 21%)
* 1x **Hora de soporte** ($40,00, IVA 4%)

*Importe base:* $65,00 | *Impuestos (15% medio):* $6,85 | *Total:* **$71,85**

![15.png](images/15.png)
*Imagen 15: Elaboración del borrador de presupuesto.*

![17.png](images/17.png)
*Imagen 17: Detalle del pedido de venta `S00026` antes de facturar.*

---

### 3. Facturación y Registro de Pago
Se crea el borrador de la factura a partir del pedido de venta, se valida y se genera la factura `INV/2026/00010`.

![18.png](images/18.png)
*Imagen 18: Borrador de la factura de cliente.*

![19.png](images/19.png)
*Imagen 19: Personalización y configuración del diseño de los documentos de venta.*

![20.png](images/20.png)
*Imagen 20: Vista previa del archivo PDF final de la factura `INV/2026/00010`.*

Se registra el pago de la factura, mostrando la etiqueta verde de **PAGADO**.

![22.png](images/22.png)
*Imagen 22: Factura en estado Registrado con el sello de PAGADO.*

---

### 4. Facturación de Proveedores y Registros Adicionales

![23.png](images/23.png)
*Imagen 23: Registro de la factura de venta `INV/2026/00008` (LightsUp).*

![24.png](images/24.png)
*Imagen 24: Factura de proveedor `FACTU/2026/11/0001` (Ready Mat).*

![25.png](images/25.png)
*Imagen 25: Historial y notas internas en la factura `INV/2026/00008`.*

---

## 5. Ejercicio 3: Búsquedas Guardadas y Filtros

En el apartado de **Facturación → Facturas**, se aplican los filtros para obtener solo las facturas publicadas, agrupadas por cliente en vista de lista, y se guarda la vista dentro del menú **Favoritos**.

---

## 6. Ejercicio 4: Modo Desarrollador

El modo desarrollador se activa mediante dos vías principales:
1. Desde **Ajustes → Herramientas de desarrollador → Activar el modo desarrollador**.
2. Añadiendo `?debug=1` a la URL (por ejemplo: `http://localhost:8069/odoo?debug=1`).

### Tabla de Nombres Técnicos
| Elemento | Campo / Modelo | Nombre Técnico |
| :--- | :--- | :--- |
| Formulario de Contacto | Campo NIF / CIF | `vat` |
| Formulario de Contacto | Modelo de datos | `res.partner` |
| Factura de Cliente | Modelo de datos | `account.move` |
| Línea de Factura | Modelo de datos | `account.move.line` |

---

## 7. Incidencias y Solución de Problemas

| Fecha | Síntoma / Problema                                         | Causa                                                            | Solución |
| :--- |:-----------------------------------------------------------|:-----------------------------------------------------------------| :--- |
| 10/11/2026 | Los cambios en los archivos no se reflejaban al reiniciar. | Los archivos en VS Code no se estaban guardando automáticamente. | Se guardaron manualmente los archivos antes de levantar la pila Docker. |
| 10/11/2026 | Error al entrar a pgAdmin 4 con el usuario                 | Contraseña erronea                                               | Poner la contraseña correcta |
| 10/11/2026 | Poner el 21% de IVA por defecto en vez del 15%             | Venia por defecto                                                |I guess well never know

---

## 8. Declaración del Uso de Inteligencia Artificial

- Le mandé mis códigos a la IA para ver el motivo por el cual no funcionaba. Después de hablar un poco y **no** cambiar el código, llegué a la conclusión (sin ayuda de la IA) de que VS Code no guarda los archivos constantemente, sino que hay que guardarlos manualmente.
- Le pedí a la IA que me ayudara a crear un README.md para explicar el proyecto y los pasos que seguí para completarlo. La IA me ayudó a redactar el README.md, pero no me dio ninguna solución a los problemas que tuve con el código.