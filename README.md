# Guía de Instalación de Odoo 18 con Docker Compose

Esta guía completa te llevará paso a paso por el proceso de instalación y configuración de un entorno de desarrollo Odoo 18 utilizando Docker, VS Code y PgAdmin. Está diseñada para personas que están comenzando y necesitan instrucciones detalladas.

## 📋 Tabla de Contenidos

1. [Requisitos Previos](#requisitos-previos)
2. [Paso 1: Preparar el IDE (VS Code)](#paso-1-preparar-el-ide-vs-code)
3. [Paso 2: Instalar Odoo 18 y PgAdmin con Docker Compose](#paso-2-instalar-odoo-18-y-pgadmin-con-docker-compose)
4. [Paso 3: Configurar Odoo con Datos de Demostración](#paso-3-configurar-odoo-con-datos-de-demostración)
5. [Paso 4: Explorar la Base de Datos con PgAdmin](#paso-4-explorar-la-base-de-datos-con-pgadmin)

---

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Docker Desktop**: Para ejecutar contenedores (puedes descargarlo desde [docker.com](https://www.docker.com/))
- **VS Code** (o PyCharm si lo prefieres): Editor de código
- **Git**: Para clonar este repositorio (opcional)

---

## Paso 1: Preparar el IDE (VS Code)

### 1.1 Instalar VS Code

1. Descarga VS Code desde [code.visualstudio.com](https://code.visualstudio.com/)
2. Instala el programa siguiendo las instrucciones del instalador
3. Abre VS Code

### 1.2 Instalar Extensiones Útiles

Para trabajar con Docker, Python y Odoo, necesitarás instalar algunas extensiones. Ve a la sección de extensiones (icono de cuadrados en la barra lateral izquierda) y busca e instala las siguientes:

| Extensión | Utilidad |
|-----------|----------|
| **Docker** | Gestionar contenedores, imágenes y Docker Compose directamente desde VS Code |
| **Python** | Soporte completo para Python (autocompletado, depuración, linting) - esencial para Odoo |
| **Pylance** | Mejora el IntelliSense y análisis de código Python |
| **Dev Containers** | Permite desarrollar dentro de contenedores Docker |
| **YAML** | Ayuda con la sintaxis del archivo docker-compose.yml |
| **PostgreSQL** (opcional) | Facilita la conexión y consultas a bases de datos PostgreSQL |

![Extensiones instaladas en VS Code](img.png)

### 1.3 Verificar Docker

Asegúrate de que Docker Desktop esté ejecutándose. Puedes verificarlo abriendo una terminal en VS Code y ejecutando:

```bash
docker --version
docker-compose --version
```

---

## Paso 2: Instalar Odoo 18 y PgAdmin con Docker Compose

### 2.1 Contenido del archivo docker-compose.yml

Este archivo define tres servicios que trabajarán juntos:

```yaml
services:

  odoo:
    image: odoo:18 #
    container_name: odoo
    ports:
      - "8069:8069" # Usamos el puerto recomendado por Odoo
    depends_on: # Aseguramos que Odoo se inicie después de la base de datos
      - db
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
    volumes:
      - odoo_data:/var/lib/odoo

  db:
    image: postgres:16 # Usaramos la version 16 de Postgres
    container_name: odoo_db
    environment:
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_DB=postgres
    volumes:
      - db_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4:latest # Imagen oficial de pgAdmin
    restart: unless-stopped
    depends_on:
      - db # Aseguramos que pgAdmin se inicie después de la base de datos
    environment:
      - PGADMIN_DEFAULT_EMAIL=danifv02@gmail.com
      - PGADMIN_DEFAULT_PASSWORD=Abc123.
    ports:
      - "8081:80" # Mapeamos el puerto 80 del contenedor al 8081 del host

volumes:
  odoo_data: # Volumen para persistir datos de Odoo
  db_data: # Volumen para persistir datos de PostgreSQL
```

**Explicación de los servicios:**

- **odoo**: Contenedor con Odoo 18, accesible en `http://localhost:8069`
- **db**: Base de datos PostgreSQL 16 que utilizará Odoo
- **pgadmin**: Interfaz web para administrar PostgreSQL, accesible en `http://localhost:8081`

### 2.2 Levantar los Contenedores

1. Abre una terminal en la carpeta donde guardaste el archivo `docker-compose.yml`
2. Ejecuta el siguiente comando:

```bash
docker-compose up -d
```

Este comando descargará las imágenes necesarias y levantará los contenedores en segundo plano.

![Terminal ejecutando docker-compose up](img_1.png)

### 2.3 Verificar que los Contenedores Están Corriendo

Puedes verificar que todos los contenedores estén ejecutándose con:

```bash
docker ps
```

O usando la extensión de Docker en VS Code, donde verás los tres contenedores activos.

![Docker Desktop mostrando los contenedores activos](img_2.png)

---

## Paso 3: Configurar Odoo con Datos de Demostración

### 3.1 Acceder a Odoo

1. Abre tu navegador web
2. Ve a `http://localhost:8069`
3. Verás la pantalla de configuración inicial de Odoo

![Pantalla inicial de Odoo](img_3.png)

### 3.2 Crear la Base de Datos con Datos de Demostración

Completa el formulario de creación de base de datos:

1. **Master Password**: Deja la contraseña por defecto o crea una nueva (guárdala bien)
2. **Database Name**: Elige un nombre (ej: `odoo_demo`)
3. **Email**: Tu email (será el usuario administrador)
4. **Password**: Contraseña para el usuario administrador
5. **Phone Number**: Opcional
6. **Language**: Español / Spanish
7. **Country**: España (o tu país)
8. **⚠️ IMPORTANTE**: Marca la casilla **"Load demonstration data"** (Cargar datos de demostración)

![Formulario de creación de base de datos](img_4.png)

Haz clic en "Create Database" y espera a que se complete el proceso (puede tardar unos minutos).

### 3.3 Acceder al Panel de Odoo

Una vez creada la base de datos, serás redirigido al panel principal de Odoo.

![Panel principal de Odoo](img_5.png)

### 3.4 Instalar Módulos Básicos

1. Ve al menú **Aplicaciones** (Apps)
2. Verás una lista de módulos disponibles
3. Instala los módulos básicos que necesites, por ejemplo:
   - **Ventas** (Sales)
   - **Compras** (Purchase)
   - **Contactos** (Contacts)
   - **Inventario** (Inventory)
   - **Facturación** (Invoicing)

![Menú de aplicaciones en Odoo](img_6.png)

4. Haz clic en "Install" o "Activate" en cada módulo
5. Espera a que se instalen (puede tardar unos minutos)

![Instalando un módulo](img_7.png)

### 3.5 Explorar los Datos de Demostración

Una vez instalados los módulos, podrás navegar por ellos y ver datos de ejemplo:

- **Contactos**: Clientes y proveedores de ejemplo
- **Productos**: Catálogo de productos de muestra
- **Ventas**: Presupuestos y pedidos de venta de ejemplo
- **Compras**: Órdenes de compra de ejemplo

![Explorando datos de demostración en Odoo](img_8.png)

Estos datos te permitirán familiarizarte con el funcionamiento de Odoo sin tener que crear todo desde cero.

![Vista de módulos con datos de demostración](img_9.png)

---

## Paso 4: Explorar la Base de Datos con PgAdmin

### 4.1 Acceder a PgAdmin

1. Abre tu navegador web
2. Ve a `http://localhost:8081`
3. Verás la pantalla de inicio de sesión de PgAdmin

![Pantalla de inicio de PgAdmin](img_10.png)

4. Inicia sesión con las credenciales del `docker-compose.yml`:
   - **Email**: `danifv02@gmail.com`
   - **Password**: `Abc123.`

### 4.2 Conectar con el Servidor PostgreSQL

1. Una vez dentro, haz clic derecho en "Servers" en el panel izquierdo
2. Selecciona **"Register" > "Server"**
3. En la pestaña **"General"**:
   - **Name**: Odoo DB (o el nombre que prefieras)
4. En la pestaña **"Connection"**:
   - **Host name/address**: `db` (nombre del servicio en docker-compose)
   - **Port**: `5432` (puerto por defecto de PostgreSQL)
   - **Maintenance database**: `postgres`
   - **Username**: `odoo`
   - **Password**: `odoo`
   - Marca "Save password" si quieres

![Configuración de conexión en PgAdmin](img_11.png)

5. Haz clic en "Save"

### 4.3 Inspeccionar la Base de Datos de Odoo

1. En el panel izquierdo, expande:
   - **Servers** > **Odoo DB** > **Databases**
2. Verás la base de datos que creaste (ej: `odoo_demo`)
3. Expande la base de datos y luego **Schemas** > **public** > **Tables**
4. Aquí verás todas las tablas que Odoo ha creado

![Explorando las tablas de Odoo en PgAdmin](img_12.png)

Algunas tablas importantes:
- `res_partner`: Contactos (clientes, proveedores)
- `product_product`: Productos
- `sale_order`: Órdenes de venta
- `purchase_order`: Órdenes de compra
- `account_move`: Asientos contables

Puedes hacer clic derecho en cualquier tabla y seleccionar **"View/Edit Data" > "First 100 Rows"** para ver los datos que Odoo ha almacenado.

---

## 🎉 ¡Listo!

Ahora tienes un entorno de desarrollo Odoo 18 completo funcionando con:
- ✅ Odoo 18 Community en `http://localhost:8069`
- ✅ PostgreSQL 16 como base de datos
- ✅ PgAdmin para administrar la base de datos en `http://localhost:8081`
- ✅ Datos de demostración para explorar y aprender

### Comandos Útiles

Para **detener** los contenedores:
```bash
docker-compose down
```

Para **reiniciar** los contenedores:
```bash
docker-compose up -d
```

Para **ver los logs** de Odoo:
```bash
docker logs odoo -f
```

### Notas Importantes

- Los datos se persisten en volúmenes de Docker, por lo que no perderás tu información al reiniciar los contenedores
- Si quieres empezar de cero, puedes eliminar los volúmenes con `docker-compose down -v` (⚠️ esto borrará todos los datos)
- Odoo está programado en Python, por lo que las extensiones de Python en tu IDE te serán muy útiles si quieres desarrollar módulos personalizados

---

## 📚 Recursos Adicionales

- [Documentación oficial de Odoo](https://www.odoo.com/documentation/18.0/)
- [Documentación de Docker](https://docs.docker.com/)
- [Documentación de PostgreSQL](https://www.postgresql.org/docs/)

---

**Autor**: Daniel Figueroa  
**Fecha**: 2024  
**Versión**: Odoo 18 Community

