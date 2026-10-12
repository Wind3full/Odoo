# Configuración segura de Odoo con Docker

## 1. Diferencias entre `compose.yml` y `odoo.conf`

- **`compose.yml`:** configura los contenedores, los puertos, los volúmenes, las imágenes y las variables de entorno.
- **`odoo.conf`:** configura el funcionamiento interno de Odoo, como las rutas de los módulos, los registros y el directorio de datos.

**¿Qué es más seguro?** Ninguno es seguro por sí solo. Es importante no guardar contraseñas en archivos que se suban a GitHub. Podemos utilizar un archivo `.env` excluido del repositorio o Docker Secrets para gestionar los datos sensibles.

**¿Qué es más práctico para desarrollar en local?** Utilizar Docker Compose junto con un archivo `odoo.conf` y un `.env` local permite modificar la configuración de forma sencilla sin cambiar el código.

## 2. Diferencias entre desarrollo y producción

En desarrollo, se utiliza `--dev=all` para facilitar las pruebas, modificar módulos y disponer de herramientas de depuración. También se pueden utilizar puertos locales y una configuración flexible para programar.

En producción, no se utiliza el modo de desarrollo, sino una configuración estable y controlada. Se emplean registros controlados, herramientas de monitorización, acceso protegido mediante HTTPS y un proxy inverso. Además, se utilizan contraseñas seguras y se limitan los recursos para mejorar la seguridad y el rendimiento.

Podemos tener ambas opciones configuradas mediante tres archivos: `compose.yml` para la configuración común, `compose.dev.yml` para desarrollo y `compose.prod.yml` para producción.

## 3. ¿Qué directorios de Odoo podemos subir a GitHub?

- **`addons/`:** sí, si contiene módulos propios, porque es código fuente. No debemos incluir contraseñas ni datos privados.
- **`filestore/`:** no, porque puede contener documentos y archivos privados de los usuarios.
- **`sessions/`:** no, porque contiene información temporal de las sesiones de los usuarios.
- **Datos de PostgreSQL:** tampoco deben subirse al repositorio. Se deben guardar en volúmenes y copias de seguridad.

## 4. Configuración creada

He preparado una configuración de Docker Compose que separa el entorno de desarrollo del de producción. También incluye un archivo `odoo.conf`, una plantilla `.env.example` y un `.gitignore` para evitar subir contraseñas y datos privados.

Los datos de Odoo y PostgreSQL se almacenan en volúmenes para conservarlos cuando se recrean los contenedores.

## 5. Cómo iniciar los servicios

### Desarrollo

```bash
docker compose -f compose.yml -f compose.dev.yml up -d
```

Esta configuración activa el modo de desarrollo y permite trabajar con los módulos locales.

### Producción

```bash
docker compose -f compose.yml -f compose.prod.yml up -d
```

Esta configuración evita activar el modo de desarrollo y no publica el puerto de PostgreSQL en el anfitrión.

Antes de iniciar los servicios, hay que crear el archivo `.env` a partir de `.env.example` y cambiar la contraseña de ejemplo por una segura.

## 6. Seguridad

El archivo `.env` no debe subirse a GitHub, por lo que se incluye en `.gitignore`. El archivo `.env.example` sí puede subirse, ya que sirve como plantilla sin credenciales reales.

En producción también es importante utilizar HTTPS, realizar copias de seguridad, mantener las imágenes actualizadas y comprobar que los datos se pueden restaurar correctamente.