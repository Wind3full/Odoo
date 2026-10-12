## 1. Diferencias entre compose.yml y odoo.conf
- compose.yml: configura los contenedores, los puertos, los volúmenes, las imágenes y las variables de entorno.
- odoo.conf: configura el funcionamiento interno de Odoo, como las rutas de los módulos, los registros, la base de datos y los límites de recursos.

¿Qué es más seguro? Ninguno es seguro por sí solo. Lo importante es no guardar contraseñas en texto plano dentro de archivos que se suban a GitHub. Podemos utilizar un archivo .env excluido del repositorio o Docker Secrets para gestionar los datos sensibles.

¿Qué es más práctico para desarrollar en local? Utilizar Docker Compose junto con un archivo odoo.conf y un .env local. Así podemos cambiar la configuración sin modificar continuamente el código.

## 2. Diferencias entre desarrollo y producción
En desarrollo, se utiliza --dev=all para facilitar las pruebas, modificar y probar módulos y disponer de herramientas de depuración. También se pueden utilizar puertos locales y una configuración más flexible y cómoda para programar.

En producción, no se utiliza el modo de desarrollo, sino una configuración estable y controlada. Se emplean registros controlados y herramientas de monitorización, acceso protegido mediante HTTPS y un proxy inverso, contraseñas seguras y límites de recursos para mejorar la seguridad y el rendimiento.

Configuración cómoda para programar.	Contraseñas seguras y recursos limitados.
Podemos tener ambas opciones con dos archivos: compose.yml para la configuración común y compose.dev.yml para desarrollo. Para producción utilizaremos compose.prod.yml.

## 3. ¿Qué directorios de Odoo podemos subir a GitHub?
- addons/: sí, si contiene módulos propios. Es código fuente y conviene mantenerlo versionado.
- filestore/: no. Puede contener documentos y archivos privados de los usuarios.
- sessions/: no. Contiene información temporal de las sesiones de los usuarios.
- Datos de PostgreSQL: tampoco. Deben guardarse en volúmenes y copias de seguridad.
