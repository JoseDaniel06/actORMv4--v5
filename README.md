# Introducción
El propósito de este documento es proporcionar las instrucciones necesarias para actualizar el esquema del ORM de la versión 4 a la versión 5 Esta nueva versión incluye importantes mejoras y nuevas funcionalidades que serán de gran ayuda para optimizar los procesos operativos de su empresa.

En este README, encontrarán una guía detallada paso a paso sobre las acciones que deben llevar a cabo en su base de datos para implementar la versión actualizada del ORM. Además, se incluyen explicaciones sobre los cambios realizados, las mejoras introducidas y los aspectos que deben tener en cuenta durante el proceso de actualización.

# Instalación ORM en la base de datos
1. Descargar todos los recursos del repositorio
***
2. Con el fin de instalar cada una de los release correspondientes a versión 4 debe aplicar los scrips en el siguiente orden:
* 1. Versión 4.2
* 2. Versión 4.2.0.4
* 3. Versión 4.2.0.5
* 4. Versión 4.2.0.7
* 5. Versión 5.0.0.0
***
3. Se debe tener en cuenta que para la aplicación de estos scripts en su esquema ORM de su base de datos debe de estar conectado con el usuario ORM. Debe validar que los tablespaces correspondan a los mismos del script. De lo contrario debe actualizar el script con los tablespace contenidos en su Base de Datos.
***
4. Para no generar errores de compilación se recomienda validar por cada script aplicado, es decir se debe generar un reporte de base de datos
