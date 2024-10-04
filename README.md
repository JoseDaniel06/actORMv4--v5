# Introducción
El propósito de este documento es proporcionar las instrucciones necesarias para actualizar el esquema del ORM de la versión 4 a la versión 5 Esta nueva versión incluye importantes mejoras y nuevas funcionalidades que serán de gran ayuda para optimizar los procesos operativos de su empresa.

En este README, encontrarán una guía detallada paso a paso sobre las acciones que deben llevar a cabo en su base de datos para implementar la versión actualizada del ORM. Además, se incluyen explicaciones sobre los cambios realizados, las mejoras introducidas y los aspectos que deben tener en cuenta durante el proceso de actualización.

# Instalación ORM en la base de datos
1. Descargar todos los recursos del repositorio
***
2. Antes de correr cada script por favor editarlo para realizar los ajustes que deban hacerle según sus políticas de creación de usuarios, seguridad y estructura a nivel de base de datos
*** 
3. Con el fin de instalar cada una de los release correspondientes a versión 4 debe aplicar los scrips en el siguiente orden:
* 1. Versión 4.2 (No instalar Package)
* 2. Versión 4.2.0.4
* 3. Versión 4.2.0.5
* 4. Versión 4.2.0.7
* 5. Versión 5.0.0.0
***
4. Se debe tener en cuenta que para la aplicación de estos scripts en su esquema ORM de su base de datos debe de estar conectado con el usuario ORM. Debe validar que los tablespaces correspondan a los mismos del script. De lo contrario debe actualizar el script con los tablespace contenidos en su Base de Datos.
***
5. Para no generar errores de compilación se recomienda validar por cada script aplicado, es decir se debe generar un reporte de las aplicacioes realizadas.
   Este es un script para la validación de los objetos aplicados y su estado.
```
SELECT OBJECT_TYPE ELEMENTO, COUNT(*) CANTIDAD
FROM USER_OBJECTS
GROUP BY OBJECT_TYPE
UNION
SELECT 'CONSTRAINT ' || DECODE(CONSTRAINT_TYPE, 'R', 'REFERENTIAL', 'P', 'PRIMARY', 'C', 'CHECK', CONSTRAINT_TYPE) ELEMENTO, COUNT(*) CANTIDAD
FROM USER_CONSTRAINTS
GROUP BY CONSTRAINT_TYPE
UNION
SELECT  'OBJETOS INVALIDOS' ELEMENTO, COUNT(*) CANTIDAD
         FROM USER_OBJECTS A
        WHERE STATUS = 'INVALID'
          AND OBJECT_TYPE IN
                 ('PACKAGE BODY', 'PACKAGE', 'FUNCTION', 'PROCEDURE',
                  'TRIGGER', 'VIEW')
ORDER BY 1;
```

   El siguiente script sirve para validar las tablas existentes al esquema y su respectivo TABLESPACE. Se recomienda desplegarlo con salida DBMS

```
DECLARE
   CURSOR table_cur IS
      SELECT OWNER, TABLE_NAME, TABLESPACE_NAME
      FROM ALL_TABLES
      WHERE OWNER = 'ORM'
      ORDER BY TABLE_NAME;
   
   -- Variables para almacenar los valores de cada fila del cursor
   v_owner ALL_TABLES.OWNER%TYPE;
   v_table_name ALL_TABLES.TABLE_NAME%TYPE;
   v_tablespace_name ALL_TABLES.TABLESPACE_NAME%TYPE;

BEGIN
   -- Abrir el cursor y recorrer cada fila
   OPEN table_cur;
   LOOP
      FETCH table_cur INTO v_owner, v_table_name, v_tablespace_name;
      EXIT WHEN table_cur%NOTFOUND;

      -- Mostrar los resultados en la consola
      DBMS_OUTPUT.PUT_LINE('Propietario: ' || v_owner);
      DBMS_OUTPUT.PUT_LINE('Tabla: ' || v_table_name);
      DBMS_OUTPUT.PUT_LINE('Tablespace: ' || v_tablespace_name);
      DBMS_OUTPUT.PUT_LINE('--------------------------------------------');
   END LOOP;
   CLOSE table_cur;
END;
/
```
***
6. Probablemente saldrán errores de tablespace, tabla, vista o campo que ya existe porque se está realizando una actualización de versión por lo cual no es un error.
***
7. Si en la ejecución sale un error de primary key es la misma situación del punto anterior. Por lo que en este paso se recomienda (insertar script que valide objetos existentes y solo inserte los que no existen).
***
8. Por último se realiza la instalación del paquete v4.2.0.4 ----Validar
***
9. Como nota adicional se debe tener en cuenta que la instalación debe ser en el siguiente orden 01 luego 02 ... y así sucesivamente.
***
10. En este paso se deben validar que todos los scripts estén compilados correctamente, uno seguido del otro.