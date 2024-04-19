```sql


Tabla Departamento
CREATE TABLE departamento(
	`codigo` INT AUTO_INCREMENT,
    `nombre` VARCHAR(100),
	`presupuesto` DOUBLE,
    `gastos` DOUBLE,
    CONSTRAINT PK_departamento PRIMARY KEY (`codigo`)
);

mysql> DESCRIBE departamento;

+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| codigo      | int          | NO   | PRI | NULL    | auto_increment |
| nombre      | varchar(100) | YES  |     | NULL    |                |
| presupuesto | double       | YES  |     | NULL    |                |
| gastos      | double       | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+


tabla Empleado
CREATE TABLE empleado(
	`codigo` INT,
    `nit` VARCHAR(9),
    `nombre` VARCHAR(100),
    `apellido1` VARCHAR(100),
    `apellido2` VARCHAR(100),
    `codigo_departamento` INT(10),
    CONSTRAINT PK_empleado PRIMARY KEY (`codigo`),
    CONSTRAINT FK_codigo_departamento 
    FOREIGN KEY (`codigo_departamento`) 
    REFERENCES departamento(`codigo`) 
);

mysql> DESCRIBE empleado;
+---------------------+--------------+------+-----+---------+-------+
| Field               | Type         | Null | Key | Default | Extra |
+---------------------+--------------+------+-----+---------+-------+
| codigo              | int          | NO   | PRI | NULL    |       |
| nit                 | varchar(9)   | YES  |     | NULL    |       |
| nombre              | varchar(100) | YES  |     | NULL    |       |
| apellido1           | varchar(100) | YES  |     | NULL    |       |
| apellido2           | varchar(100) | YES  |     | NULL    |       |
| codigo_departamento | int          | YES  | MUL | NULL    |       |
+---------------------+--------------+------+-----+---------+-------+



```



```sql


INSERT INTO departamento(`codigo`, `nombre`, `presupuesto`,`gastos`) VALUES
(1, 'Desarrollo', 120000, 6000),
(2, 'Sistemas', 150000, 21000),
(3, 'Recursos Humanos', 280000, 25000),
(4, 'Contabilidad', 110000, 3000),
(5, 'I+D', 375000, 380000),
(6, 'Proyectos', 0, 0),
(7, 'Publicidad', 0, 1000);


```

```sql

INSERT INTO empleado(
    `codigo`,
    `nit`,
    `nombre`, 
    `apellido1`, 
    `apellido2`, 
    `codigo_departamento`
) VALUES
    (1, '32481596F', 'Aarón', 'Rivero', 'Gómez', 1),
    (2, 'Y5575632D', 'Adela', 'Salas', 'Díaz', 2),
    (3, 'R6970642B', 'Adolfo', 'Rubio', 'Flores', 3),
    (4, '77705545E', 'Adrián', 'Suárez', NULL, 4),
    (5, '17087203C', 'Marcos', 'Loyola', 'Méndez', 5),
    (6, '38382980M', 'María', 'Santana', 'Moreno', 1),
    (7, '80576669X', 'Pilar', 'Ruiz', NULL, 2),
    (8, '71651431Z', 'Pepe', 'Ruiz', 'Santana', 3),
    (9, '56399183D', 'Juan', 'Gómez', 'López', 2),
    (10, '46384486H', 'Diego','Flores', 'Salas', 5),
    (11, '67389283A', 'Marta','Herrera', 'Gil', 1),
    (12, '41234836R', 'Irene','Salas', 'Flores', NULL),
    (13, '82635162B', 'Juan Antonio','Sáez', 'Guerrero', NULL);


```

## Consultas sobre una tabla 

1. Lista el primer apellido de todos los empleados.

   ```sql
   SELECT 
   	e.apellido1 AS 'PRIMER APELLIDO' 
   FROM 
   	empleado AS e;
   
   +-----------------+
   | PRIMER APELLIDO |
   +-----------------+
   | Rivero          |
   | Salas           |
   | Rubio           |
   | Suárez          |
   | Loyola          |
   | Santana         |
   | Ruiz            |
   | Ruiz            |
   | Gómez           |
   | Flores          |
   | Herrera         |
   | Salas           |
   | Sáez            |
   +-----------------+
   13 rows in set (0.00 sec)
   ```

2. Lista el primer apellido de los empleados eliminando los apellidos que estén
   repetidos.

   ````sql
   SELECT DISTINCT
   	e.apellido1 AS 'PRIMER APELLIDO' 
   FROM 
   	empleado AS e;
   	
   +-----------------+
   | PRIMER APELLIDO |
   +-----------------+
   | Rivero          |
   | Salas           |
   | Rubio           |
   | Suárez          |
   | Loyola          |
   | Santana         |
   | Ruiz            |
   | Gómez           |
   | Flores          |
   | Herrera         |
   | Sáez            |
   +-----------------+
   11 rows in set (0.00 sec)
   ````

3. Lista todas las columnas de la tabla empleado.

   ````sql
   SELECT
   	e.codigo,
   	e.nit,
   	e.nombre,
   	e.apellido1,
   	e.apellido2,
       e.codigo_departamento
   FROM 
   	empleado AS e;
   	
   +--------+-----------+--------------+-----------+-----------+---------------------+
   | codigo | nit       | nombre       | apellido1 | apellido2 | codigo_departamento |
   +--------+-----------+--------------+-----------+-----------+---------------------+
   |      1 | 32481596F | Aarón        | Rivero    | Gómez     |                   1 |
   |      2 | Y5575632D | Adela        | Salas     | Díaz      |                   2 |
   |      3 | R6970642B | Adolfo       | Rubio     | Flores    |                   3 |
   |      4 | 77705545E | Adrián       | Suárez    | NULL      |                   4 |
   |      5 | 17087203C | Marcos       | Loyola    | Méndez    |                   5 |
   |      6 | 38382980M | María        | Santana   | Moreno    |                   1 |
   |      7 | 80576669X | Pilar        | Ruiz      | NULL      |                   2 |
   |      8 | 71651431Z | Pepe         | Ruiz      | Santana   |                   3 |
   |      9 | 56399183D | Juan         | Gómez     | López     |                   2 |
   |     10 | 46384486H | Diego        | Flores    | Salas     |                   5 |
   |     11 | 67389283A | Marta        | Herrera   | Gil       |                   1 |
   |     12 | 41234836R | Irene        | Salas     | Flores    |                NULL |
   |     13 | 82635162B | Juan Antonio | Sáez      | Guerrero  |                NULL |
   +--------+-----------+--------------+-----------+-----------+---------------------+
   13 rows in set (0.00 sec)
   ````

4. Lista el nombre y los apellidos de todos los empleados.

   `````sql
   SELECT
   	e.nombre,
   	e.apellido1,
   	e.apellido2
   FROM 
   	empleado AS e;
   	
   +--------------+-----------+-----------+
   | nombre       | apellido1 | apellido2 |
   +--------------+-----------+-----------+
   | Aarón        | Rivero    | Gómez     |
   | Adela        | Salas     | Díaz      |
   | Adolfo       | Rubio     | Flores    |
   | Adrián       | Suárez    | NULL      |
   | Marcos       | Loyola    | Méndez    |
   | María        | Santana   | Moreno    |
   | Pilar        | Ruiz      | NULL      |
   | Pepe         | Ruiz      | Santana   |
   | Juan         | Gómez     | López     |
   | Diego        | Flores    | Salas     |
   | Marta        | Herrera   | Gil       |
   | Irene        | Salas     | Flores    |
   | Juan Antonio | Sáez      | Guerrero  |
   +--------------+-----------+-----------+
   13 rows in set (0.00 sec)
   `````

5. Lista el identificador de los departamentos de los empleados que aparecen
   en la tabla empleado.

   ````sql
   SELECT
   	e.codigo_departamento
   FROM 
   	empleado AS e;
   	
   +---------------------+
   | codigo_departamento |
   +---------------------+
   |                NULL |
   |                NULL |
   |                   1 |
   |                   1 |
   |                   1 |
   |                   2 |
   |                   2 |
   |                   2 |
   |                   3 |
   |                   3 |
   |                   4 |
   |                   5 |
   |                   5 |
   +---------------------+
   13 rows in set (0.00 sec)
   ````

6. Lista el identificador de los departamentos de los empleados que aparecen
   en la tabla empleado, eliminando los identificadores que aparecen repetidos.

   `````sql
   SELECT DISTINCT 
   	e.codigo_departamento
   FROM 
   	empleado AS e;
   	
   +---------------------+
   | codigo_departamento |
   +---------------------+
   |                NULL |
   |                   1 |
   |                   2 |
   |                   3 |
   |                   4 |
   |                   5 |
   +---------------------+
   6 rows in set (0.00 sec)
   `````

7. Lista el nombre y apellidos de los empleados en una única columna.

   `````sql
   SELECT CONCAT 
   	(e.nombre,' ',
   	 e.apellido1,' ',
   	 e.apellido2) AS 'NOMBRE Y APELLIDOS'
   FROM 
   	empleado AS e;
   	
   +----------------------------+
   | NOMBRE Y APELLIDOS         |
   +----------------------------+
   | Aarón Rivero Gómez         |
   | Adela Salas Díaz           |
   | Adolfo Rubio Flores        |
   | NULL                       |
   | Marcos Loyola Méndez       |
   | María Santana Moreno       |
   | NULL                       |
   | Pepe Ruiz Santana          |
   | Juan Gómez López           |
   | Diego Flores Salas         |
   | Marta Herrera Gil          |
   | Irene Salas Flores         |
   | Juan Antonio Sáez Guerrero |
   +----------------------------+
   13 rows in set (0.00 sec)
   `````

8. Lista el nombre y apellidos de los empleados en una única columna,
   convirtiendo todos los caracteres en mayúscula.

   ````sql
   SELECT UPPER(CONCAT 
   	(e.nombre,' ',
   	 e.apellido1,' ',
   	 e.apellido2)) AS 'NOMBRE Y APELLIDOS'
   FROM 
   	empleado AS e;
   	
   +----------------------------+
   | NOMBRE Y APELLIDOS         |
   +----------------------------+
   | AARÓN RIVERO GÓMEZ         |
   | ADELA SALAS DÍAZ           |
   | ADOLFO RUBIO FLORES        |
   | NULL                       |
   | MARCOS LOYOLA MÉNDEZ       |
   | MARÍA SANTANA MORENO       |
   | NULL                       |
   | PEPE RUIZ SANTANA          |
   | JUAN GÓMEZ LÓPEZ           |
   | DIEGO FLORES SALAS         |
   | MARTA HERRERA GIL          |
   | IRENE SALAS FLORES         |
   | JUAN ANTONIO SÁEZ GUERRERO |
   +----------------------------+
   13 rows in set (0.00 sec)
   ````

9. Lista el nombre y apellidos de los empleados en una única columna,
   convirtiendo todos los caracteres en minúscula.

   ````sql
   SELECT LOWER(CONCAT 
   	(e.nombre,' ',
   	 e.apellido1,' ',
   	 e.apellido2)) AS 'NOMBRE Y APELLIDOS'
   FROM 
   	empleado AS e;
   
   +----------------------------+
   | NOMBRE Y APELLIDOS         |
   +----------------------------+
   | aarón rivero gómez         |
   | adela salas díaz           |
   | adolfo rubio flores        |
   | NULL                       |
   | marcos loyola méndez       |
   | maría santana moreno       |
   | NULL                       |
   | pepe ruiz santana          |
   | juan gómez lópez           |
   | diego flores salas         |
   | marta herrera gil          |
   | irene salas flores         |
   | juan antonio sáez guerrero |
   +----------------------------+
   13 rows in set (0.00 sec)
   ````

10. Lista el identificador de los empleados junto al nif, pero el nif deberá
    aparecer en dos columnas, una mostrará únicamente los dígitos del nif y la
    otra la letra.

    ````sql
    SELECT 
    	e.codigo,
    	SUBSTRING(e.nit, 1, LENGTH(e.nit) - 1) AS 'Dígitos',
        RIGHT(e.nit, 1) AS 'Letra'
    FROM 
    	empleado AS e;
    	
    +--------+----------+-------+
    | codigo | Dígitos  | Letra |
    +--------+----------+-------+
    |      1 | 32481596 | F     |
    |      2 | Y5575632 | D     |
    |      3 | R6970642 | B     |
    |      4 | 77705545 | E     |
    |      5 | 17087203 | C     |
    |      6 | 38382980 | M     |
    |      7 | 80576669 | X     |
    |      8 | 71651431 | Z     |
    |      9 | 56399183 | D     |
    |     10 | 46384486 | H     |
    |     11 | 67389283 | A     |
    |     12 | 41234836 | R     |
    |     13 | 82635162 | B     |
    +--------+----------+-------+
    13 rows in set (0.00 sec)
    ````

11. Lista el nombre de cada departamento y el valor del presupuesto actual del
    que dispone. Para calcular este dato tendrá que restar al valor del
    presupuesto inicial (columna presupuesto) los gastos que se han generado
    (columna gastos). Tenga en cuenta que en algunos casos pueden existir
    valores negativos. Utilice un alias apropiado para la nueva columna columna
    que está calculando.

    ````sql
    SELECT
    	d.nombre,
    	(d.presupuesto - d.gastos) AS 'presupuesto actual'
    FROM 
    	departamento AS d
    	
    +------------------+-------------+
    | nombre           | presupuesto |
    +------------------+-------------+
    | Desarrollo       |      114000 |
    | Sistemas         |      129000 |
    | Recursos Humanos |      255000 |
    | Contabilidad     |      107000 |
    | I+D              |       -5000 |
    | Proyectos        |           0 |
    | Publicidad       |       -1000 |
    +------------------+-------------+
    7 rows in set (0.00 sec)	
    ````

12. Lista el nombre de los departamentos y el valor del presupuesto actual
    ordenado de forma ascendente.

    ````sql
    SELECT
    	d.nombre,
    	(d.presupuesto - d.gastos) AS 'presupuesto actual'
    FROM 
    	departamento AS d
    ORDER BY 
    	d.nombre ASC;
    +------------------+--------------------+
    | nombre           | presupuesto actual |
    +------------------+--------------------+
    | Contabilidad     |             107000 |
    | Desarrollo       |             114000 |
    | I+D              |              -5000 |
    | Proyectos        |                  0 |
    | Publicidad       |              -1000 |
    | Recursos Humanos |             255000 |
    | Sistemas         |             129000 |
    +------------------+--------------------+
    7 rows in set (0.00 sec)
    ````

13. Lista el nombre de todos los departamentos ordenados de forma
    ascendente.

    ````sql
    SELECT
    	d.nombre
    FROM 
    	departamento AS d
    ORDER BY 
    	d.nombre ASC;
    	
    +------------------+
    | nombre           |
    +------------------+
    | Contabilidad     |
    | Desarrollo       |
    | I+D              |
    | Proyectos        |
    | Publicidad       |
    | Recursos Humanos |
    | Sistemas         |
    +------------------+
    7 rows in set (0.00 sec)
    ````

14. Lista el nombre de todos los departamentos ordenados de forma
    descendente.

    ````sql
    SELECT
    	d.nombre AS 'Nombre Departamento'
    FROM 
    	departamento AS d
    ORDER BY 
    	d.nombre DESC;
    	
    +---------------------+
    | Nombre Departamento |
    +---------------------+
    | Sistemas            |
    | Recursos Humanos    |
    | Publicidad          |
    | Proyectos           |
    | I+D                 |
    | Desarrollo          |
    | Contabilidad        |
    +---------------------+
    7 rows in set (0.00 sec)
    ````

15. Lista los apellidos y el nombre de todos los empleados, ordenados de forma
    alfabética tendiendo en cuenta en primer lugar sus apellidos y luego su
    nombre.

    ````sql
    SELECT CONCAT(
        e.apellido1, ' ',
        e.apellido2, ' ',
        e.nombre) AS 'Nombre Empleado'
    FROM empleado AS e
    ORDER BY e.apellido1 ASC;
    
    +----------------------------+
    | Nombre Empleado            |
    +----------------------------+
    | Flores Salas Diego         |
    | Gómez López Juan           |
    | Herrera Gil Marta          |
    | Loyola Méndez Marcos       |
    | Rivero Gómez Aarón         |
    | Rivero Gomez Aaron         |
    | Rubio Flores Adolfo        |
    | NULL                       |
    | Ruiz Santana Pepe          |
    | Sáez Guerrero Juan Antonio |
    | Salas Díaz Adela           |
    | Salas Flores Irene         |
    | Santana Moreno María       |
    | NULL                       |
    +----------------------------+
    14 rows in set (0.00 sec)
    ````

16. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos
    que tienen mayor presupuesto.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    ORDER BY
    	d.presupuesto DESC
    LIMIT 3;
    
    +------------------+-------------+
    | nombre           | presupuesto |
    +------------------+-------------+
    | I+D              |      375000 |
    | Recursos Humanos |      280000 |
    | Sistemas         |      150000 |
    +------------------+-------------+
    3 rows in set (0.00 sec)
    ````

17. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos
    que tienen menor presupuesto.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    ORDER BY
    	d.presupuesto ASC
    LIMIT 3;
    
    +--------------+-------------+
    | nombre       | presupuesto |
    +--------------+-------------+
    | Proyectos    |           0 |
    | Publicidad   |           0 |
    | Contabilidad |      110000 |
    +--------------+-------------+
    3 rows in set (0.00 sec)
    ````

18. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que
    tienen mayor gasto.

    ````sql
    SELECT 
    	d.nombre,
    	d.gastos
    FROM 
    	departamento AS d
    ORDER BY
    	d.gastos DESC
    LIMIT 2;
    
    +------------------+--------+
    | nombre           | gastos |
    +------------------+--------+
    | I+D              | 380000 |
    | Recursos Humanos |  25000 |
    +------------------+--------+
    2 rows in set (0.00 sec)
    
    ````

19. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que
    tienen menor gasto.

    ````sql
    SELECT 
    	d.nombre,
    	d.gastos
    FROM 
    	departamento AS d
    ORDER BY
    	d.gastos ASC
    LIMIT 2;
    
    +------------+--------+
    | nombre     | gastos |
    +------------+--------+
    | Proyectos  |      0 |
    | Publicidad |   1000 |
    +------------+--------+
    2 rows in set (0.00 sec)
    ````

20. Devuelve una lista con 5 filas a partir de la tercera fila de la tabla empleado. La
    tercera fila se debe incluir en la respuesta. La respuesta debe incluir todas las
    columnas de la tabla empleado.

    ````sql
    SELECT 
    	e.codigo,
    	e.nombre
    FROM 
    	empleado AS e
    LIMIT 5 OFFSET 2;
    
    +--------+--------+
    | codigo | nombre |
    +--------+--------+
    |      3 | Adolfo |
    |      4 | Adrián |
    |      5 | Marcos |
    |      6 | María  |
    |      7 | Pilar  |
    +--------+--------+
    5 rows in set (0.00 sec)
    ````

21. Devuelve una lista con el nombre de los departamentos y el presupuesto, de
    aquellos que tienen un presupuesto mayor o igual a 150000 euros.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    WHERE 
    	d.presupuesto >= 150000;
    
    +------------------+-------------+
    | nombre           | presupuesto |
    +------------------+-------------+
    | Sistemas         |      150000 |
    | Recursos Humanos |      280000 |
    | I+D              |      375000 |
    +------------------+-------------+
    3 rows in set (0.00 sec)
    ````

22. Devuelve una lista con el nombre de los departamentos y el gasto, de
    aquellos que tienen menos de 5000 euros de gastos.

    ````sql
    SELECT 
    	d.nombre,
    	d.gastos
    FROM 
    	departamento AS d
    WHERE 
    	d.gastos < 5000;
    	
    +--------------+--------+
    | nombre       | gastos |
    +--------------+--------+
    | Contabilidad |   3000 |
    | Proyectos    |      0 |
    | Publicidad   |   1000 |
    +--------------+--------+
    3 rows in set (0.00 sec)
    ````

23. Devuelve una lista con el nombre de los departamentos y el presupuesto, de
    aquellos que tienen un presupuesto entre 100000 y 200000 euros. Sin
    utilizar el operador BETWEEN.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    WHERE 
    	d.presupuesto <= 200000
    	AND
    	d.presupuesto >= 100000;
    	
    +--------------+-------------+
    | nombre       | presupuesto |
    +--------------+-------------+
    | Desarrollo   |      120000 |
    | Sistemas     |      150000 |
    | Contabilidad |      110000 |
    +--------------+-------------+
    3 rows in set (0.00 sec)
    ````

24. Devuelve una lista con el nombre de los departamentos que no tienen un
    presupuesto entre 100000 y 200000 euros. Sin utilizar el operador BETWEEN.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    WHERE 
    	d.presupuesto BETWEEN 100000 AND 200000;
    	
    +--------------+-------------+
    | nombre       | presupuesto |
    +--------------+-------------+
    | Desarrollo   |      120000 |
    | Sistemas     |      150000 |
    | Contabilidad |      110000 |
    +--------------+-------------+
    3 rows in set (0.00 sec)
    ````

25. Devuelve una lista con el nombre de los departamentos que tienen un
    presupuesto entre 100000 y 200000 euros. Utilizando el operador BETWEEN.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    WHERE 
    	d.presupuesto BETWEEN 100000 AND 200000;
    	
    +--------------+-------------+
    | nombre       | presupuesto |
    +--------------+-------------+
    | Desarrollo   |      120000 |
    | Sistemas     |      150000 |
    | Contabilidad |      110000 |
    +--------------+-------------+
    3 rows in set (0.00 sec)
    ````

26. Devuelve una lista con el nombre de los departamentos que no tienen un
    presupuesto entre 100000 y 200000 euros. Utilizando el operador BETWEEN.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto
    FROM 
    	departamento AS d
    WHERE 
    	d.presupuesto NOT BETWEEN 100000 AND 200000;
    	
    +------------------+-------------+
    | nombre           | presupuesto |
    +------------------+-------------+
    | Recursos Humanos |      280000 |
    | I+D              |      375000 |
    | Proyectos        |           0 |
    | Publicidad       |           0 |
    +------------------+-------------+
    4 rows in set (0.00 sec)
    ````

27. Devuelve una lista con el nombre de los departamentos, gastos y
    presupuesto, de aquellos departamentos donde los gastos sean mayores
    que el presupuesto del que disponen.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto,
    	d.gastos
    FROM 
    	departamento AS d
    WHERE 
    	d.gastos > d.presupuesto;
    	
    +------------+-------------+--------+
    | nombre     | presupuesto | gastos |
    +------------+-------------+--------+
    | I+D        |      375000 | 380000 |
    | Publicidad |           0 |   1000 |
    +------------+-------------+--------+
    2 rows in set (0.00 sec)
    ````

28. Devuelve una lista con el nombre de los departamentos, gastos y
    presupuesto, de aquellos departamentos donde los gastos sean menores
    que el presupuesto del que disponen.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto,
    	d.gastos
    FROM 
    	departamento AS d
    WHERE 
    	d.gastos < d.presupuesto;
    	
    +------------------+-------------+--------+
    | nombre           | presupuesto | gastos |
    +------------------+-------------+--------+
    | Desarrollo       |      120000 |   6000 |
    | Sistemas         |      150000 |  21000 |
    | Recursos Humanos |      280000 |  25000 |
    | Contabilidad     |      110000 |   3000 |
    +------------------+-------------+--------+
    4 rows in set (0.00 sec)
    ````

29. Devuelve una lista con el nombre de los departamentos, gastos y
    presupuesto, de aquellos departamentos donde los gastos sean iguales al
    presupuesto del que disponen.

    ````sql
    SELECT 
    	d.nombre,
    	d.presupuesto,
    	d.gastos
    FROM 
    	departamento AS d
    WHERE 
    	d.gastos = d.presupuesto;
    	
    +-----------+-------------+--------+
    | nombre    | presupuesto | gastos |
    +-----------+-------------+--------+
    | Proyectos |           0 |      0 |
    +-----------+-------------+--------+
    1 row in set (0.00 sec)
    ````

30. Lista todos los datos de los empleados cuyo segundo apellido sea NULL.

    ````sql
    SELECT
    	e.codigo,
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2,
        e.codigo_departamento
    FROM 
    	empleado AS e
    WHERE 
    	e.apellido2 IS NULL;
    
    +--------+-----------+--------+-----------+-----------+---------------------+
    | codigo | nit       | nombre | apellido1 | apellido2 | codigo_departamento |
    +--------+-----------+--------+-----------+-----------+---------------------+
    |      4 | 77705545E | Adrián | Suárez    | NULL      |                   4 |
    |      7 | 80576669X | Pilar  | Ruiz      | NULL      |                   2 |
    +--------+-----------+--------+-----------+-----------+---------------------+
    2 rows in set (0.00 sec)
    ````

31. Lista todos los datos de los empleados cuyo segundo apellido no sea NULL.

    ````sql
    SELECT
    	e.codigo,
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2,
        e.codigo_departamento
    FROM 
    	empleado AS e
    WHERE 
    	e.apellido2 IS NOT NULL;
    	
    +--------+-----------+--------------+-----------+-----------+---------------------+
    | codigo | nit       | nombre       | apellido1 | apellido2 | codigo_departamento |
    +--------+-----------+--------------+-----------+-----------+---------------------+
    |      1 | 32481596F | Aarón        | Rivero    | Gómez     |                   1 |
    |      2 | Y5575632D | Adela        | Salas     | Díaz      |                   2 |
    |      3 | R6970642B | Adolfo       | Rubio     | Flores    |                   3 |
    |      5 | 17087203C | Marcos       | Loyola    | Méndez    |                   5 |
    |      6 | 38382980M | María        | Santana   | Moreno    |                   1 |
    |      8 | 71651431Z | Pepe         | Ruiz      | Santana   |                   3 |
    |      9 | 56399183D | Juan         | Gómez     | López     |                   2 |
    |     10 | 46384486H | Diego        | Flores    | Salas     |                   5 |
    |     11 | 67389283A | Marta        | Herrera   | Gil       |                   1 |
    |     12 | 41234836R | Irene        | Salas     | Flores    |                NULL |
    |     13 | 82635162B | Juan Antonio | Sáez      | Guerrero  |                NULL |
    +--------+-----------+--------------+-----------+-----------+---------------------+
    11 rows in set (0.00 sec)
    ````

32. Lista todos los datos de los empleados cuyo segundo apellido sea López.

    ````sql
    SELECT
    	e.codigo,
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2,
        e.codigo_departamento
    FROM 
    	empleado AS e
    WHERE 
    	e.apellido2 LIKE 'López';
    ````

33. Lista todos los datos de los empleados cuyo segundo apellido
    sea Díaz o Moreno. Sin utilizar el operador IN.

    ````sql
    SELECT
    	e.codigo,
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2,
        e.codigo_departamento
    FROM 
    	empleado AS e
    WHERE 
    	e.apellido2 = 'Díaz'
    	OR
    	e.apellido2 = 'Moreno';
    	
    +--------+-----------+--------+-----------+-----------+---------------------+
    | codigo | nit       | nombre | apellido1 | apellido2 | codigo_departamento |
    +--------+-----------+--------+-----------+-----------+---------------------+
    |      2 | Y5575632D | Adela  | Salas     | Díaz      |                   2 |
    |      6 | 38382980M | María  | Santana   | Moreno    |                   1 |
    +--------+-----------+--------+-----------+-----------+---------------------+
    2 rows in set (0.00 sec)
    ````

34. Lista todos los datos de los empleados cuyo segundo apellido
    sea Díaz o Moreno. Utilizando el operador IN.

    ````sql
    SELECT
    	e.codigo,
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2,
        e.codigo_departamento
    FROM 
    	empleado AS e
    WHERE 
    	e.apellido2 IN ('Díaz', 'Moreno');
    	
    +--------+-----------+--------+-----------+-----------+---------------------+
    | codigo | nit       | nombre | apellido1 | apellido2 | codigo_departamento |
    +--------+-----------+--------+-----------+-----------+---------------------+
    |      2 | Y5575632D | Adela  | Salas     | Díaz      |                   2 |
    |      6 | 38382980M | María  | Santana   | Moreno    |                   1 |
    +--------+-----------+--------+-----------+-----------+---------------------+
    2 rows in set (0.00 sec)
    ````

35. Lista los nombres, apellidos y nif de los empleados que trabajan en el
    departamento 3.

    ````sql
    SELECT
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2
    FROM 
    	empleado AS e
    WHERE 
    	e.codigo_departamento = 3;
    	
    +-----------+--------+-----------+-----------+
    | nit       | nombre | apellido1 | apellido2 |
    +-----------+--------+-----------+-----------+
    | R6970642B | Adolfo | Rubio     | Flores    |
    | 71651431Z | Pepe   | Ruiz      | Santana   |
    +-----------+--------+-----------+-----------+
    2 rows in set (0.00 sec)
    ````

36. Lista los nombres, apellidos y nif de los empleados que trabajan en los
    departamentos 2, 4 o 5.

    ````sql
    SELECT
    	e.nit,
    	e.nombre,
    	e.apellido1,
    	e.apellido2
    FROM 
    	empleado AS e
    WHERE 
    	e.codigo_departamento IN (2, 4, 5);
    	
    +-----------+--------+-----------+-----------+
    | nit       | nombre | apellido1 | apellido2 |
    +-----------+--------+-----------+-----------+
    | Y5575632D | Adela  | Salas     | Díaz      |
    | 80576669X | Pilar  | Ruiz      | NULL      |
    | 56399183D | Juan   | Gómez     | López     |
    | 77705545E | Adrián | Suárez    | NULL      |
    | 17087203C | Marcos | Loyola    | Méndez    |
    | 46384486H | Diego  | Flores    | Salas     |
    +-----------+--------+-----------+-----------+
    6 rows in set (0.00 sec)
    ````



## Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2.

1. Devuelve un listado con los empleados y los datos de los departamentos
   donde trabaja cada uno.

   ````sql
   SELECT 
       e.nit,
       e.nombre,
       d.codigo,
       d.nombre,
       d.presupuesto,
       d.gastos
   FROM 
       empleado AS e
   JOIN 
       departamento AS d ON e.codigo_departamento = d.codigo;
       
   +-----------+--------+--------+------------------+-------------+--------+
   | nit       | nombre | codigo | nombre           | presupuesto | gastos |
   +-----------+--------+--------+------------------+-------------+--------+
   | 32481596F | Aarón  |      1 | Desarrollo       |      120000 |   6000 |
   | 38382980M | María  |      1 | Desarrollo       |      120000 |   6000 |
   | 67389283A | Marta  |      1 | Desarrollo       |      120000 |   6000 |
   | Y5575632D | Adela  |      2 | Sistemas         |      150000 |  21000 |
   | 80576669X | Pilar  |      2 | Sistemas         |      150000 |  21000 |
   | 56399183D | Juan   |      2 | Sistemas         |      150000 |  21000 |
   | R6970642B | Adolfo |      3 | Recursos Humanos |      280000 |  25000 |
   | 71651431Z | Pepe   |      3 | Recursos Humanos |      280000 |  25000 |
   | 77705545E | Adrián |      4 | Contabilidad     |      110000 |   3000 |
   | 17087203C | Marcos |      5 | I+D              |      375000 | 380000 |
   | 46384486H | Diego  |      5 | I+D              |      375000 | 380000 |
   +-----------+--------+--------+------------------+-------------+--------+
   11 rows in set (0.00 sec)
   
   ````

2. Devuelve un listado con los empleados y los datos de los departamentos
   donde trabaja cada uno. Ordena el resultado, en primer lugar por el nombre
   del departamento (en orden alfabético) y en segundo lugar por los apellidos
   y el nombre de los empleados.

   ````sql
   SELECT 
   	e.nit,
       e.nombre,
       d.codigo,
       d.nombre,
       d.presupuesto,
       d.gastos
   FROM
   	empleado AS e
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   ORDER BY
   	d.nombre ASC,
   	e.nombre ASC;
   	
   +-----------+--------+--------+------------------+-------------+--------+
   | nit       | nombre | codigo | nombre           | presupuesto | gastos |
   +-----------+--------+--------+------------------+-------------+--------+
   | 77705545E | Adrián |      4 | Contabilidad     |      110000 |   3000 |
   | 32481596F | Aarón  |      1 | Desarrollo       |      120000 |   6000 |
   | 38382980M | María  |      1 | Desarrollo       |      120000 |   6000 |
   | 67389283A | Marta  |      1 | Desarrollo       |      120000 |   6000 |
   | 46384486H | Diego  |      5 | I+D              |      375000 | 380000 |
   | 17087203C | Marcos |      5 | I+D              |      375000 | 380000 |
   | R6970642B | Adolfo |      3 | Recursos Humanos |      280000 |  25000 |
   | 71651431Z | Pepe   |      3 | Recursos Humanos |      280000 |  25000 |
   | Y5575632D | Adela  |      2 | Sistemas         |      150000 |  21000 |
   | 56399183D | Juan   |      2 | Sistemas         |      150000 |  21000 |
   | 80576669X | Pilar  |      2 | Sistemas         |      150000 |  21000 |
   +-----------+--------+--------+------------------+-------------+--------+
   11 rows in set (0.00 sec)
   ````

3. Devuelve un listado con el identificador y el nombre del departamento,
   solamente de aquellos departamentos que tienen empleados.

   ````sql
   SELECT 
   	d.codigo,
   	d.nombre
   FROM 
   	departamento AS d
   JOIN
   	empleado AS e ON d.codigo = e.codigo_departamento 
   ````

4. Devuelve un listado con el identificador, el nombre del departamento y el
   valor del presupuesto actual del que dispone, solamente de aquellos
   departamentos que tienen empleados. El valor del presupuesto actual lo
   puede calcular restando al valor del presupuesto inicial
   (columna presupuesto) el valor de los gastos que ha generado
   (columna gastos).

   ````sql
   SELECT DISTINCT 
   	 d.nombre,
   	(d.presupuesto - d.gastos) AS 'Presupuesto Actual'
   FROM 
   	departamento AS d 
   JOIN
   	empleado AS e ON e.codigo_departamento = d.codigo ; 
   	
   +------------------+--------------------+
   | nombre           | Presupuesto Actual |
   +------------------+--------------------+
   | Desarrollo       |             114000 |
   | Sistemas         |             129000 |
   | Recursos Humanos |             255000 |
   | Contabilidad     |             107000 |
   | I+D              |              -5000 |
   +------------------+--------------------+
   5 rows in set (0,00 sec)
   
   ````

5. Devuelve el nombre del departamento donde trabaja el empleado que tiene
   el nif 38382980M.

   `````sql
   SELECT 
   	 d.nombre
   FROM 
   	empleado AS e 
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   WHERE 
   	e.nit = '38382980M';
   
   
   +------------+
   | nombre     |
   +------------+
   | Desarrollo |
   +------------+
   1 row in set (0,00 sec)
   
   `````

6. Devuelve el nombre del departamento donde trabaja el empleado Pepe Ruiz
   Santana.

   ````sql
   SELECT 
   	 d.nombre
   FROM 
   	empleado AS e 
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   WHERE 
   	CONCAT(
   		e.nombre, ' ',
   		e.apellido1, ' ',
   		e.apellido2 
   	) LIKE 'Pepe Ruiz Santana';
   
   +------------------+
   | nombre           |
   +------------------+
   | Recursos Humanos |
   +------------------+
   1 row in set (0,00 sec)
   
   ````

7. Devuelve un listado con los datos de los empleados que trabajan en el
   departamento de I+D. Ordena el resultado alfabéticamente.

   ````sql
   SELECT 
   	 e.nit,
   	 e.nombre 
   FROM 
   	empleado AS e
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   WHERE 
   	d.nombre = 'I+D'
   ORDER BY
   	e.nombre ASC;
   	
   +-----------+--------+
   | nit       | nombre |
   +-----------+--------+
   | 46384486H | Diego  |
   | 17087203C | Marcos |
   +-----------+--------+
   2 rows in set (0,00 sec)
   
   ````

8. Devuelve un listado con los datos de los empleados que trabajan en el
   departamento de Sistemas, Contabilidad o I+D. Ordena el resultado
   alfabéticamente.

   ````sql
   SELECT 
   	 e.nit,
   	 e.nombre 
   FROM 
   	empleado AS e
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   WHERE 
   	d.nombre IN ('Sistemas', 'Contabilidad', 'I+D')
   ORDER BY
   	e.nombre ASC;
   	
   +-----------+---------+
   | nit       | nombre  |
   +-----------+---------+
   | Y5575632D | Adela   |
   | 77705545E | Adrián  |
   | 46384486H | Diego   |
   | 56399183D | Juan    |
   | 17087203C | Marcos  |
   | 80576669X | Pilar   |
   +-----------+---------+
   6 rows in set (0,00 sec)
   
   ````

9. Devuelve una lista con el nombre de los empleados que tienen los
   departamentos que no tienen un presupuesto entre 100000 y 200000 euros.

   ````sql
   SELECT 
   	 e.nombre 
   FROM 
   	empleado AS e
   JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo
   WHERE 
   	d.presupuesto BETWEEN 100000 AND 200000;
   
   +---------+
   | nombre  |
   +---------+
   | Aarón   |
   | María   |
   | Marta   |
   | Adela   |
   | Pilar   |
   | Juan    |
   | Adrián  |
   +---------+
   7 rows in set (0,00 sec)
   
   ````

10. Devuelve un listado con el nombre de los departamentos donde existe
    algún empleado cuyo segundo apellido sea NULL. Tenga en cuenta que no
    debe mostrar nombres de departamentos que estén repetidos.

    ````sql
    SELECT DISTINCT 
    	 d.nombre 
    FROM 
    	departamento AS d
    JOIN
    	empleado AS e ON e.codigo_departamento = d.codigo
    WHERE 
    	e.apellido2 IS NULL;
    	
    +--------------+
    | nombre       |
    +--------------+
    | Contabilidad |
    | Sistemas     |
    +--------------+
    2 rows in set (0,00 sec)
    
    ````



## Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

1. Devuelve un listado con todos los empleados junto con los datos de los
   departamentos donde trabajan. Este listado también debe incluir los
   empleados que no tienen ningún departamento asociado.

   ````sql
   SELECT
   	e.nombre, 
   	d.nombre 
   FROM 
   	empleado AS e
   LEFT JOIN
   	departamento AS d ON d.codigo = e.codigo_departamento; 
   	
   +--------------+------------------+
   | nombre       | nombre           |
   +--------------+------------------+
   | Aarón        | Desarrollo       |
   | Adela        | Sistemas         |
   | Adolfo       | Recursos Humanos |
   | Adrián       | Contabilidad     |
   | Marcos       | I+D              |
   | María        | Desarrollo       |
   | Pilar        | Sistemas         |
   | Pepe         | Recursos Humanos |
   | Juan         | Sistemas         |
   | Diego        | I+D              |
   | Marta        | Desarrollo       |
   | Irene        | NULL             |
   | Juan Antonio | NULL             |
   +--------------+------------------+
   13 rows in set (0,00 sec)
   
   
   ````

2. Devuelve un listado donde sólo aparezcan aquellos empleados que no
   tienen ningún departamento asociado.

   ````sql
   SELECT
   	e.nit,
   	e.nombre,
   	d.nombre
   FROM 
   	empleado AS e
   LEFT JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo 
   WHERE
   	e.codigo_departamento IS NULL
   	
   +-----------+--------------+--------+
   | nit       | nombre       | nombre |
   +-----------+--------------+--------+
   | 41234836R | Irene        | NULL   |
   | 82635162B | Juan Antonio | NULL   |
   +-----------+--------------+--------+
   2 rows in set (0,00 sec)
   
   ````

3. Devuelve un listado donde sólo aparezcan aquellos departamentos que no
   tienen ningún empleado asociado.

   ````sql
   SELECT
   	e.nombre AS 'Nombre Empleado',
   	d.nombre AS 'Nombre Departamento'
   FROM 
   	empleado AS e
   RIGHT JOIN
   	departamento AS d ON e.codigo_departamento = d.codigo 
   WHERE
   	e.codigo_departamento IS NULL;
   	
   +-----------------+---------------------+
   | Nombre Empleado | Nombre Departamento |
   +-----------------+---------------------+
   | NULL            | Proyectos           |
   | NULL            | Publicidad          |
   +-----------------+---------------------+
   2 rows in set (0,00 sec)
   
   ````

4. Devuelve un listado con todos los empleados junto con los datos de los
   departamentos donde trabajan. El listado debe incluir los empleados que no
   tienen ningún departamento asociado y los departamentos que no tienen
   ningún empleado asociado. Ordene el listado alfabéticamente por el
   nombre del departamento.

   ````sql
   /*MySQL*/
   SELECT 
   	e.nombre,
   	d.nombre AS 'Nombre Departamento'
   FROM 
   	empleado AS e 
   RIGHT JOIN
   	departamento d ON d.codigo = e.codigo_departamento
   UNION
   SELECT 
   	e.nombre,
   	d.nombre AS 'Nombre Departamento'
   FROM 
   	empleado AS e 
   LEFT JOIN
   	departamento d ON d.codigo = e.codigo_departamento;
   	
   +--------------+---------------------+
   | nombre       | Nombre Departamento |
   +--------------+---------------------+
   | Aarón        | Desarrollo          |
   | María        | Desarrollo          |
   | Marta        | Desarrollo          |
   | Adela        | Sistemas            |
   | Pilar        | Sistemas            |
   | Juan         | Sistemas            |
   | Adolfo       | Recursos Humanos    |
   | Pepe         | Recursos Humanos    |
   | Adrián       | Contabilidad        |
   | Marcos       | I+D                 |
   | Diego        | I+D                 |
   | NULL         | Proyectos           |
   | NULL         | Publicidad          |
   | Irene        | NULL                |
   | Juan Antonio | NULL                |
   +--------------+---------------------+
   15 rows in set (0,00 sec)
   
   ````

5. Devuelve un listado con los empleados que no tienen ningún departamento
   asociado y los departamentos que no tienen ningún empleado asociado.
   Ordene el listado alfabéticamente por el nombre del departamento.

   ````sql
   SELECT 
   	e.nombre,
   	d.nombre AS 'Nombre Departamento'
   FROM 
   	empleado AS e 
   LEFT JOIN
   	departamento d ON d.codigo = e.codigo_departamento
   WHERE
   	d.codigo IS NULL
   UNION
   SELECT 
   	e.nombre,
   	d.nombre AS 'Nombre Departamento'
   FROM 
   	empleado AS e 
   RIGHT JOIN
   	departamento d ON d.codigo = e.codigo_departamento
   WHERE
   	e.codigo_departamento IS NULL;
   	
   +--------------+---------------------+
   | nombre       | Nombre Departamento |
   +--------------+---------------------+
   | Irene        | NULL                |
   | Juan Antonio | NULL                |
   | NULL         | Proyectos           |
   | NULL         | Publicidad          |
   +--------------+---------------------+
   4 rows in set (0,00 sec)
   
   ````



## Consultas resumen

1. Calcula la suma del presupuesto de todos los departamentos.

   ````sql
   SELECT 
   	SUM(d.presupuesto) AS 'Total Presupuesto'
   FROM
   	departamento AS d;
   	
   +-------------------+
   | Total Presupuesto |
   +-------------------+
   |           1035000 |
   +-------------------+
   1 row in set (0,00 sec)
   
   ````

2. Calcula la media del presupuesto de todos los departamentos.

   ````sql
   SELECT 
   	ROUND(AVG(d.presupuesto)) AS 'Promedio Pesupuesto'
   FROM
   	departamento AS d;
   
   +---------------------+
   | Promedio Pesupuesto |
   +---------------------+
   |              147857 |
   +---------------------+
   1 row in set (0,00 sec)
   
   
   ````

3. Calcula el valor mínimo del presupuesto de todos los departamentos.

   ````sql
   SELECT 
   	MIN(d.presupuesto) AS 'Presupuesto Minimo'
   FROM
   	departamento AS d;
   	
   +--------------------+
   | Presupuesto Minimo |
   +--------------------+
   |                  0 |
   +--------------------+
   1 row in set (0,01 sec)
   
   ````

4. Calcula el nombre del departamento y el presupuesto que tiene asignado,
   del departamento con menor presupuesto.

   ````sql
   SELECT 
       d.nombre AS 'Nombre del Departamento',
       d.presupuesto AS 'Presupuesto'
   FROM 
       departamento AS d
   WHERE 
       d.presupuesto = (SELECT MIN(presupuesto) FROM departamento);
       
   +-------------------------+-------------+
   | Nombre del Departamento | Presupuesto |
   +-------------------------+-------------+
   | Proyectos               |           0 |
   | Publicidad              |           0 |
   +-------------------------+-------------+
   2 rows in set (0,00 sec)
   
   ````

5. Calcula el valor máximo del presupuesto de todos los departamentos.

   ````sql
   SELECT 
   	MAX(d.presupuesto) AS 'Presupuesto Maximo'
   FROM
   	departamento AS d ;
   
   +--------------------+
   | Presupuesto Maximo |
   +--------------------+
   |             375000 |
   +--------------------+
   1 row in set (0,00 sec)
   
   ````

6. Calcula el nombre del departamento y el presupuesto que tiene asignado,
   del departamento con mayor presupuesto.

   ````sql
   SELECT 
       d.nombre AS 'Nombre del Departamento',
       d.presupuesto AS 'Presupuesto'
   FROM 
       departamento AS d
   WHERE 
       d.presupuesto = (SELECT MAX(presupuesto) FROM departamento);
       
   +-------------------------+-------------+
   | Nombre del Departamento | Presupuesto |
   +-------------------------+-------------+
   | I+D                     |      375000 |
   +-------------------------+-------------+
   1 row in set (0,00 sec)
   ````

7. Calcula el número total de empleados que hay en la tabla empleado.

   ````sql
   SELECT 
   	COUNT(e.codigo) AS 'Total Empleados'
   FROM
   	empleado AS e;
   
   +-----------------+
   | Total Empleados |
   +-----------------+
   |              13 |
   +-----------------+
   1 row in set (0,00 sec)
   
   ````

8. Calcula el número de empleados que no tienen NULL en su segundo
   apellido.

   ````sql
   SELECT 
   	COUNT(e.codigo) AS 'Empleados Con dos Apellidos'
   FROM
   	empleado AS e
   WHERE 
   	e.apellido2 IS NOT NULL;
   	
   +-----------------------------+
   | Empleados Con dos Apellidos |
   +-----------------------------+
   |                          11 |
   +-----------------------------+
   1 row in set (0,00 sec)
   
   ````

9. Calcula el número de empleados que hay en cada departamento. Tienes que
   devolver dos columnas, una con el nombre del departamento y otra con el
   número de empleados que tiene asignados.

   ````sql
   SELECT 
       d.nombre AS 'Nombre del Departamento',
       COUNT(e.codigo) AS 'Número de Empleados'
   FROM 
       departamento d
   LEFT JOIN 
       empleado e ON d.codigo = e.codigo_departamento
   GROUP BY 
       d.codigo, d.nombre;
       
   +-------------------------+----------------------+
   | Nombre del Departamento | Número de Empleados  |
   +-------------------------+----------------------+
   | Desarrollo              |                    3 |
   | Sistemas                |                    3 |
   | Recursos Humanos        |                    2 |
   | Contabilidad            |                    1 |
   | I+D                     |                    2 |
   | Proyectos               |                    0 |
   | Publicidad              |                    0 |
   +-------------------------+----------------------+
   7 rows in set (0,00 sec)
   
   
   ````

10. Calcula el nombre de los departamentos que tienen más de 2 empleados. El
    resultado debe tener dos columnas, una con el nombre del departamento y
    otra con el número de empleados que tiene asignados.

    ````sql
    SELECT 
        d.nombre AS 'Nombre del Departamento',
        COUNT(e.codigo) AS 'Número de Empleados'
    FROM 
        departamento AS d
    LEFT JOIN 
        empleado e ON d.codigo = e.codigo_departamento
    GROUP BY 
        d.nombre
    HAVING 
    	COUNT(e.codigo) > 2;
    	
    +-------------------------+----------------------+
    | Nombre del Departamento | Número de Empleados  |
    +-------------------------+----------------------+
    | Desarrollo              |                    3 |
    | Sistemas                |                    3 |
    +-------------------------+----------------------+
    2 rows in set (0,00 sec)
    
    ````

11. Calcula el número de empleados que trabajan en cada uno de los
    departamentos. El resultado de esta consulta también tiene que incluir
    aquellos departamentos que no tienen ningún empleado asociado.

    `````sql
    SELECT 
        d.nombre AS 'Nombre del Departamento',
        COUNT(e.codigo) AS 'Número de Empleados'
    FROM 
        departamento d
    LEFT JOIN 
        empleado e ON d.codigo = e.codigo_departamento
    GROUP BY 
        d.codigo, d.nombre;
        
    +-------------------------+----------------------+
    | Nombre del Departamento | Número de Empleados  |
    +-------------------------+----------------------+
    | Desarrollo              |                    3 |
    | Sistemas                |                    3 |
    | Recursos Humanos        |                    2 |
    | Contabilidad            |                    1 |
    | I+D                     |                    2 |
    | Proyectos               |                    0 |
    | Publicidad              |                    0 |
    +-------------------------+----------------------+
    7 rows in set (0,00 sec)
    `````

12. Calcula el número de empleados que trabajan en cada unos de los
    departamentos que tienen un presupuesto mayor a 200000 euros.

    `````sql
    SELECT 
        d.nombre AS 'Nombre del Departamento',
        COUNT(e.codigo) AS 'Número de Empleados'
    FROM 
        departamento AS d
    LEFT JOIN 
        empleado e ON d.codigo = e.codigo_departamento
    WHERE 
    	d.presupuesto > 200000
    GROUP BY 
        d.nombre;
        
    +-------------------------+----------------------+
    | Nombre del Departamento | Número de Empleados  |
    +-------------------------+----------------------+
    | Recursos Humanos        |                    2 |
    | I+D                     |                    2 |
    +-------------------------+----------------------+
    2 rows in set (0,00 sec)
    
    `````



## Subconsultas

#### Con operadores básicos de comparación

1. Devuelve un listado con todos los empleados que tiene el departamento
   de Sistemas. (Sin utilizar INNER JOIN).

   `````sql
   SELECT 
   	e.nombre 
   FROM
   	empleado AS e 
   WHERE 
   	e.codigo_departamento = (
   		SELECT 
           	d.codigo 
   		FROM 
           	departamento AS d
   		WHERE 
   			d.nombre = 'Sistemas'
   	);
   							
   +--------+
   | nombre |
   +--------+
   | Adela  |
   | Pilar  |
   | Juan   |
   +--------+
   3 rows in set (0,00 sec)
   
   `````

2. Devuelve el nombre del departamento con mayor presupuesto y la cantidad
   que tiene asignada.

   ````sql
   SELECT 
   	d.nombre,
   	d.presupuesto 
   FROM
   	departamento AS d
   WHERE 
   	d.presupuesto  = (
   		SELECT MAX(d.presupuesto)
   		FROM departamento AS d
   	);
   
   +--------+-------------+
   | nombre | presupuesto |
   +--------+-------------+
   | I+D    |      375000 |
   +--------+-------------+
   1 row in set (0,00 sec)
   
   ````

3. Devuelve el nombre del departamento con menor presupuesto y la cantidad
   que tiene asignada.

   ````sql
   SELECT 
   	d.nombre,
   	d.presupuesto 
   FROM
   	departamento AS d
   WHERE 
   	d.presupuesto  = (
   		SELECT MIN(d.presupuesto)
   		FROM departamento AS d
   	);
   	
   +------------+-------------+
   | nombre     | presupuesto |
   +------------+-------------+
   | Proyectos  |           0 |
   | Publicidad |           0 |
   +------------+-------------+
   2 rows in set (0,00 sec)
   ````



#### Subconsultas con ALL y ANY

4. Devuelve el nombre del departamento con mayor presupuesto y la cantidad
   que tiene asignada. Sin hacer uso de MAX, ORDER BY ni LIMIT.

   `````sql
   SELECT 
   	d.nombre,
   	d.presupuesto 
   FROM
   	departamento AS d
   WHERE 
   	d.presupuesto > ALL (
   	  SELECT presupuesto 
   	  FROM departamento
   	  WHERE 
   	  	presupuesto > d.presupuesto 
   	);
   	
   +--------+-------------+
   | nombre | presupuesto |
   +--------+-------------+
   | I+D    |      375000 |
   +--------+-------------+
   1 row in set (0,00 sec)
   
   `````

5. Devuelve el nombre del departamento con menor presupuesto y la cantidad
   que tiene asignada. Sin hacer uso de MIN, ORDER BY ni LIMIT.

   ````sql
   SELECT 
   	d.nombre,
   	d.presupuesto 
   FROM
   	departamento AS d
   WHERE 
   	d.presupuesto < ALL (
   	  SELECT presupuesto 
   	  FROM departamento
   	  WHERE 
   	  	presupuesto < d.presupuesto 
   	);
   	
   +------------+-------------+
   | nombre     | presupuesto |
   +------------+-------------+
   | Proyectos  |           0 |
   | Publicidad |           0 |
   +------------+-------------+
   2 rows in set (0,01 sec)
   	
   ````

6. Devuelve los nombres de los departamentos que tienen empleados
   asociados. (Utilizando ALL o ANY).

   ````sql
   SELECT 
   	d.nombre
   FROM
   	departamento AS d
   WHERE 
   	d.codigo = ANY(
   	  SELECT e.codigo_departamento 
   	  FROM empleado AS e 
   	);
   	
   +------------------+
   | nombre           |
   +------------------+
   | Desarrollo       |
   | Sistemas         |
   | Recursos Humanos |
   | Contabilidad     |
   | I+D              |
   +------------------+
   5 rows in set (0,00 sec)
   
   ````

7. Devuelve los nombres de los departamentos que no tienen empleados
   asociados. (Utilizando ALL o ANY).

   ````sql
   SELECT 
       d.nombre
   FROM 
       departamento AS d
   WHERE 
   	d.codigo = ANY (
   	SELECT 
       	d.codigo
   	FROM 
   	    departamento AS d
   	LEFT JOIN 
   	    empleado e ON d.codigo = e.codigo_departamento
   	WHERE 
   		e.codigo_departamento IS NULL
   	);
   	
   +------------+
   | nombre     |
   +------------+
   | Proyectos  |
   | Publicidad |
   +------------+
   2 rows in set (0,00 sec)
   
   ````



#### Subconsultas con IN y NOT IN

8. Devuelve los nombres de los departamentos que tienen empleados
   asociados. (Utilizando IN o NOT IN).

   ````sql
   SELECT 
   	d.nombre
   FROM 
   	departamento AS d
   WHERE
   	d.codigo IN (
       	SELECT 
           	e.codigo_departamento
           FROM 
           	empleado AS e
       );
       
    +------------------+
   | nombre           |
   +------------------+
   | Desarrollo       |
   | Sistemas         |
   | Recursos Humanos |
   | Contabilidad     |
   | I+D              |
   +------------------+
   5 rows in set (0.02 sec)
   ````

9. Devuelve los nombres de los departamentos que no tienen empleados
   asociados. (Utilizando IN o NOT IN).

   ````sql
   SELECT 
   	d.nombre
   FROM 
   	departamento AS d
   WHERE
   	d.codigo NOT IN (
       	SELECT 
           	e.codigo_departamento
           FROM 
           	empleado AS e
           WHERE
           	e.codigo_departamento IS NOT NULL
       );
       
   +------------+
   | nombre     |
   +------------+
   | Proyectos  |
   | Publicidad |
   +------------+
   2 rows in set (0.00 sec)
   
   ````



#### Subconsultas con EXISTS y NOT EXISTS

10. Devuelve los nombres de los departamentos que tienen empleados
    asociados. (Utilizando EXISTS o NOT EXISTS).

    ````sql
    SELECT 
    	d.nombre
    FROM 
    	departamento AS d
    WHERE EXISTS (
        	SELECT 
            	e.codigo_departamento
            FROM 
            	empleado AS e
        	WHERE
        		e.codigo_departamento = d.codigo
        );
        
    +------------------+
    | nombre           |
    +------------------+
    | Desarrollo       |
    | Sistemas         |
    | Recursos Humanos |
    | Contabilidad     |
    | I+D              |
    +------------------+
    5 rows in set (0.00 sec)
    ````

11. Devuelve los nombres de los departamentos que tienen empleados
    asociados. (Utilizando EXISTS o NOT EXISTS).

    ````sql
    SELECT 
    	d.nombre
    FROM 
    	departamento AS d
    WHERE NOT EXISTS (
        	SELECT 
            	e.codigo_departamento
            FROM 
            	empleado AS e
        	WHERE
        		e.codigo_departamento = d.codigo
        );
        
    +------------+
    | nombre     |
    +------------+
    | Proyectos  |
    | Publicidad |
    +------------+
    2 rows in set (0.00 sec)
    ````

    