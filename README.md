# Instalación de Odoo con Docker Compose

Guía rápida para levantar Odoo usando `docker-compose`. Incluye un ejemplo mínimo, pasos para la instalación y cómo acceder a los servicios.
Archivo docker-compose.yml de ejemplo:

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


## Pasos para la instalación

### Entramos en PgAdmin a traves del puerto 8081, con nuestro email y contraseña.

![img.png](img.png)

### En PgAdmin, añadimos un nuevo servidor para conectarnos a la base de datos de Odoo.

![img_2.png](img_2.png)

### Bases de datos funcionando: 

![img_3.png](img_3.png)

### Accedemos a Odoo a traves del puerto 8069, con los datos de administrador.

![img_5.png](img_5.png)

### Odoo ya está instalado y funcionando correctamente.

![img_6.png](img_6.png)

### Módulo "Sales" instalado y funcionando:

![img_7.png](img_7.png)

### Módulo "Contacts" instalado y funcionando:

![img_8.png](img_8.png)

### Dentro del módulo "Contacts", podemos ver los contactos creados:

![img_10.png](img_10.png)

### Módulo "Events" instalado y funcionando:

![img_9.png](img_9.png)

### Tablas de la la base de datos de Odoo en PgAdmin funcionando correctamente::

![img_11.png](img_11.png)

Extensiones usadas en PyCharm:

![img_12.png](img_12.png)

