# Examen_Postgres

# **Consultas sobre una tabla**

1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.
~~~SQL
  SELECT *
  FROM pedido
  ORDER BY fecha DESC;

 id |  total  |   fecha    | id_cliente | id_comercial
----+---------+------------+------------+--------------
 16 | 2389.23 | 2019-03-11 |          1 |            5
 15 |  370.85 | 2019-03-11 |          1 |            5
 13 |  545.75 | 2019-01-25 |          6 |            1
  8 | 1983.43 | 2017-10-10 |          4 |            6
  1 |   150.5 | 2017-10-05 |          5 |            2
  3 |   65.26 | 2017-10-05 |          2 |            1
  5 |   948.5 | 2017-09-10 |          5 |            2
 12 |  3045.6 | 2017-04-25 |          2 |            1
 14 |  145.82 | 2017-02-02 |          6 |            1
  9 |  2480.4 | 2016-10-10 |          8 |            3
  2 |  270.65 | 2016-09-10 |          1 |            5
 11 |   75.29 | 2016-08-17 |          3 |            7
  4 |   110.5 | 2016-08-17 |          8 |            3
  6 |  2400.6 | 2016-07-27 |          7 |            1
  7 |    5760 | 2015-09-10 |          2 |            1
 10 |  250.45 | 2015-06-27 |          8 |            2
~~~
2. Devuelve todos los datos de los dos pedidos de mayor valor.
~~~SQL
  SELECT *
  FROM pedido
  ORDER BY total DESC
  LIMIT 2;

 id | total  |   fecha    | id_cliente | id_comercial
----+--------+------------+------------+--------------
  7 |   5760 | 2015-09-10 |          2 |            1
 12 | 3045.6 | 2017-04-25 |          2 |            1
~~~
3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.
~~~SQL
  SELECT DISTINCT id_cliente
  FROM pedido;

 id_cliente
------------
          3
          5
          4
          6
          2
          7
          1
          8
~~~
4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.
~~~SQL
  SELECT *
  FROM pedido
  WHERE total > 500 AND fecha < '2018-01-01' AND fecha > '2016-12-31';

 id |  total  |   fecha    | id_cliente | id_comercial
----+---------+------------+------------+--------------
  5 |   948.5 | 2017-09-10 |          5 |            2
  8 | 1983.43 | 2017-10-10 |          4 |            6
 12 |  3045.6 | 2017-04-25 |          2 |            1
~~~
5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.
~~~SQL
  SELECT nombre, apellido1, apellido2
  FROM comercial
  WHERE comisión BETWEEN 0.05 AND 0.11;

 |  nombre  |  apellido1  | apellido2  |
 +----------+-------------+------------+
 | Diego    |  Flores     | Salas      |            
 | Antonio  |  Vega       | Hernandez  |            
 | Alfedro  |  Ruiz       | Flores     |            
~~~
6. Devuelve el valor de la comisión de mayor valor que existe en la tabla `comercial`.
~~~SQL
  SELECT MAX(comisión)
  FROM comercial;

 |  max  |
 +-------+
 | 0.15  |           
~~~
7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido **no** es `NULL`. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.
~~~SQL
  SELECT id, nombre, apellido1, apellido2
  FROM cliente
  WHERE apellido2 IS NOT NULL
  ORDER BY apellido1, apellido2, nombre;

 id |  nombre   | apellido1 | apellido2
----+-----------+-----------+-----------
  9 | Guillermo | L¾pez     | G¾mez
  5 | Marcos    | Loyola    | MÚndez
  1 | Aar¾n     | Rivero    | G¾mez
  3 | Adolfo    | Rubio     | Flores
  8 | Pepe      | Ruiz      | Santana
  2 | Adela     | Salas     | DÝaz
 10 | Daniel    | Santana   | Loyola
  6 | MarÝa     | Santana   | Moreno
~~~
8. Devuelve un listado de los nombres de los clientes que empiezan por `A` y terminan por `n` y también los nombres que empiezan por `P`. El listado deberá estar ordenado alfabéticamente.
~~~SQL
  SELECT nombre
  FROM cliente
  WHERE nombre LIKE 'A%n' OR nombre LIKE 'P%'
  ORDER BY nombre;

 nombre
--------
 Aar¾n
 Adrißn
 Pepe
 Pilar
~~~
9. Devuelve un listado de los nombres de los clientes que **no** empiezan por `A`. El listado deberá estar ordenado alfabéticamente.
~~~SQL
  SELECT nombre
  FROM cliente
  WHERE nombre NOT LIKE 'A%'
  ORDER BY nombre;

  nombre
-----------
 Daniel
 Guillermo
 Marcos
 MarÝa
 Pepe
 Pilar
~~~
10. Devuelve un listado con los nombres de los comerciales que terminan por `el` o `o`. Tenga en cuenta que se deberán eliminar los nombres repetidos.
~~~SQL
  SELECT DISTINCT nombre
  FROM comercial
  WHERE nombre LIKE '%el' OR nombre LIKE '%o';

 nombre
---------
 Daniel
 Diego
 Antonio
 Alfredo
 Manuel
~~~

# **Consultas multitabla (Composición interna)**

Resuelva todas las consultas utilizando la sintaxis de `SQL1` y `SQL2`.

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.
~~~SQL
  SELECT DISTINCT c.id, c.nombre, c.apellido1, c.apellido2
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id;

 id | nombre | apellido1 | apellido2
----+--------+-----------+-----------
  1 | Aar¾n  | Rivero    | G¾mez
  3 | Adolfo | Rubio     | Flores
  6 | MarÝa  | Santana   | Moreno
  2 | Adela  | Salas     | DÝaz
  4 | Adrißn | Sußrez    |
  7 | Pilar  | Ruiz      |
  8 | Pepe   | Ruiz      | Santana
  5 | Marcos | Loyola    | MÚndez
~~~
2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  ORDER BY c.nombre;

 id | nombre | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial
----+--------+-----------+-----------+---------+-----------+----+---------+------------+------------+--------------
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5
  2 | Adela  | Salas     | DÝaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
  2 | Adela  | Salas     | DÝaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1
  2 | Adela  | Salas     | DÝaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1
  3 | Adolfo | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7
  4 | Adrißn | Sußrez    |           | JaÚn    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6
  5 | Marcos | Loyola    | MÚndez    | AlmerÝa |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2
  5 | Marcos | Loyola    | MÚndez    | AlmerÝa |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2
  6 | MarÝa  | Santana   | Moreno    | Cßdiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1
  6 | MarÝa  | Santana   | Moreno    | Cßdiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3
  7 | Pilar  | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1
~~~
3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.
~~~SQL
  SELECT *
  FROM pedido p
  INNER JOIN comercial c ON c.id = p.id_comercial
  ORDER BY c.nombre;

 id |  total  |   fecha    | id_cliente | id_comercial | id | nombre  | apellido1 | apellido2 | comisi¾n
----+---------+------------+------------+--------------+----+---------+-----------+-----------+----------
 16 | 2389.23 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
 15 |  370.85 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
  2 |  270.65 | 2016-09-10 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
 11 |   75.29 | 2016-08-17 |          3 |            7 |  7 | Antonio | Vega      | Hernßndez |     0.11
  7 |    5760 | 2015-09-10 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
 14 |  145.82 | 2017-02-02 |          6 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
 13 |  545.75 | 2019-01-25 |          6 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
 12 |  3045.6 | 2017-04-25 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  3 |   65.26 | 2017-10-05 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  6 |  2400.6 | 2016-07-27 |          7 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  9 |  2480.4 | 2016-10-10 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
  4 |   110.5 | 2016-08-17 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
 10 |  250.45 | 2015-06-27 |          8 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  1 |   150.5 | 2017-10-05 |          5 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  5 |   948.5 | 2017-09-10 |          5 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  8 | 1983.43 | 2017-10-10 |          4 |            6 |  6 | Manuel  | DomÝnguez | Hernßndez |     0.13
~~~
4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON c.id = p.id_cliente
  INNER JOIN comercial cc ON cc.id = p.id_comercial;

 id | nombre | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial | id | nombre  | apellido1 | apellido2 | comisi¾n
----+--------+-----------+-----------+---------+-----------+----+---------+------------+------------+--------------+----+---------+-----------+-----------+----------
  5 | Marcos | Loyola    | MÚndez    | AlmerÝa |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
  2 | Adela  | Salas     | DÝaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
  5 | Marcos | Loyola    | MÚndez    | AlmerÝa |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  7 | Pilar  | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  2 | Adela  | Salas     | DÝaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  4 | Adrißn | Sußrez    |           | JaÚn    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6 |  6 | Manuel  | DomÝnguez | Hernßndez |     0.13
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
  8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2 |  2 | Juan    | G¾mez     | L¾pez     |     0.13
  3 | Adolfo | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7 |  7 | Antonio | Vega      | Hernßndez |     0.11
  2 | Adela  | Salas     | DÝaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  6 | MarÝa  | Santana   | Moreno    | Cßdiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  6 | MarÝa  | Santana   | Moreno    | Cßdiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1 |  1 | Daniel  | Sßez      | Vega      |     0.15
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
~~~
5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año `2017`, cuya cantidad esté entre `300` € y `1000` €.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON c.id = p.id_cliente
  WHERE p.total BETWEEN 300 AND 1000 AND p.fecha > '2016-12-31' AND p.fecha < '2018-01-01';

 id | nombre | apellido1 | apellido2 | ciudad  | categoria | id | total |   fecha    | id_cliente | id_comercial
----+--------+-----------+-----------+---------+-----------+----+-------+------------+------------+--------------
  5 | Marcos | Loyola    | MÚndez    | AlmerÝa |       200 |  5 | 948.5 | 2017-09-10 |          5 |            2
~~~
6. Devuelve el nombre y los apellidos de todos los comerciales que ha participado en algún pedido realizado por `María Santana Moreno`.
~~~SQL
  SELECT DISTINCT cc.nombre, cc.apellido1, cc.apellido2
  FROM comercial cc
  INNER JOIN pedido p ON p.id_comercial = cc.id
  INNER JOIN cliente c ON p.id_cliente = c.id
  WHERE c.nombre = 'María' AND c.apellido1 = 'Santana' AND c.apellido2 = 'Moreno';

 nombre | apellido1 | apellido2
--------+-----------+-----------
 Daniel |  Saez     | Vega
~~~
7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial `Daniel Sáez Vega`.
~~~SQL
  SELECT DISTINCT c.nombre
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  INNER JOIN comercial cc ON p.id_comercial = cc.id
  WHERE cc.nombre = 'Daniel' AND cc.apellido1 = 'Sáez' AND cc.apellido2 = 'Vega';

 nombre
--------
 Adela
 María
 Pilar
~~~

# **Consultas multitabla (Composición externa)**

Resuelva todas las consultas utilizando las cláusulas `LEFT JOIN` y `RIGHT JOIN`.

1. Devuelve un listado con **todos los clientes** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.
~~~SQL
  SELECT *
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  ORDER BY c.apellido1, c.apellido2, c.nombre;

 id |  nombre   | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial
----+-----------+-----------+-----------+---------+-----------+----+---------+------------+------------+--------------
  9 | Guillermo | L¾pez     | G¾mez     | Granada |       225 |    |         |            |            |
  5 | Marcos    | Loyola    | MÚndez    | AlmerÝa |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2
  5 | Marcos    | Loyola    | MÚndez    | AlmerÝa |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2
  1 | Aar¾n     | Rivero    | G¾mez     | AlmerÝa |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5
  1 | Aar¾n     | Rivero    | G¾mez     | AlmerÝa |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5
  1 | Aar¾n     | Rivero    | G¾mez     | AlmerÝa |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5
  3 | Adolfo    | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7
  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3
  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3
  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2
  7 | Pilar     | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1
  2 | Adela     | Salas     | DÝaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
  2 | Adela     | Salas     | DÝaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1
  2 | Adela     | Salas     | DÝaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1
 10 | Daniel    | Santana   | Loyola    | Sevilla |       125 |    |         |            |            |
  6 | MarÝa     | Santana   | Moreno    | Cßdiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1
  6 | MarÝa     | Santana   | Moreno    | Cßdiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1
  4 | Adrißn    | Sußrez    |           | JaÚn    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6
~~~
2. Devuelve un listado con **todos los comerciales** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  ORDER BY cc.apellido1, cc.apellido2, cc.nombre;

 id | nombre  | apellido1 | apellido2 | comisi¾n | id |  total  |   fecha    | id_cliente | id_comercial
----+---------+-----------+-----------+----------+----+---------+------------+------------+--------------
  5 | Antonio | Carretero | Ortega    |     0.12 | 16 | 2389.23 | 2019-03-11 |          1 |            5
  5 | Antonio | Carretero | Ortega    |     0.12 |  2 |  270.65 | 2016-09-10 |          1 |            5
  5 | Antonio | Carretero | Ortega    |     0.12 | 15 |  370.85 | 2019-03-11 |          1 |            5
  6 | Manuel  | DomÝnguez | Hernßndez |     0.13 |  8 | 1983.43 | 2017-10-10 |          4 |            6
  3 | Diego   | Flores    | Salas     |     0.11 |  9 |  2480.4 | 2016-10-10 |          8 |            3
  3 | Diego   | Flores    | Salas     |     0.11 |  4 |   110.5 | 2016-08-17 |          8 |            3
  2 | Juan    | G¾mez     | L¾pez     |     0.13 |  1 |   150.5 | 2017-10-05 |          5 |            2
  2 | Juan    | G¾mez     | L¾pez     |     0.13 |  5 |   948.5 | 2017-09-10 |          5 |            2
  2 | Juan    | G¾mez     | L¾pez     |     0.13 | 10 |  250.45 | 2015-06-27 |          8 |            2
  4 | Marta   | Herrera   | Gil       |     0.14 |    |         |            |            |
  8 | Alfredo | Ruiz      | Flores    |     0.05 |    |         |            |            |
  1 | Daniel  | Sßez      | Vega      |     0.15 | 13 |  545.75 | 2019-01-25 |          6 |            1
  1 | Daniel  | Sßez      | Vega      |     0.15 | 14 |  145.82 | 2017-02-02 |          6 |            1
  1 | Daniel  | Sßez      | Vega      |     0.15 |  6 |  2400.6 | 2016-07-27 |          7 |            1
  1 | Daniel  | Sßez      | Vega      |     0.15 |  3 |   65.26 | 2017-10-05 |          2 |            1
  1 | Daniel  | Sßez      | Vega      |     0.15 |  7 |    5760 | 2015-09-10 |          2 |            1
  1 | Daniel  | Sßez      | Vega      |     0.15 | 12 |  3045.6 | 2017-04-25 |          2 |            1
  7 | Antonio | Vega      | Hernßndez |     0.11 | 11 |   75.29 | 2016-08-17 |          3 |            7
~~~
3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.
~~~SQL
  SELECT *
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  WHERE p.id_cliente IS NULL;

 id |  nombre   | apellido1 | apellido2 | ciudad  | categoria | id | total | fecha | id_cliente | id_comercial
----+-----------+-----------+-----------+---------+-----------+----+-------+-------+------------+--------------
 10 | Daniel    | Santana   | Loyola    | Sevilla |       125 |    |       |       |            |
  9 | Guillermo | L¾pez     | G¾mez     | Granada |       225 |    |       |       |            |
~~~
4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  WHERE p.id_comercial IS NULL;

 id | nombre  | apellido1 | apellido2 | comisi¾n | id | total | fecha | id_cliente | id_comercial
----+---------+-----------+-----------+----------+----+-------+-------+------------+--------------
  8 | Alfredo | Ruiz      | Flores    |     0.05 |    |       |       |            |
  4 | Marta   | Herrera   | Gil       |     0.14 |    |       |       |            |
~~~
5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  LEFT JOIN cliente c ON p.id_cliente = c.id
  WHERE p.id_comercial IS NULL OR p.id_cliente IS NULL;

 id | nombre  | apellido1 | apellido2 | comisi¾n | id | total | fecha | id_cliente | id_comercial | id | nombre | apellido1 | apellido2 | ciudad | categoria
----+---------+-----------+-----------+----------+----+-------+-------+------------+--------------+----+--------+-----------+-----------+--------+-----------
  8 | Alfredo | Ruiz      | Flores    |     0.05 |    |       |       |            |              |    |        |           |           |        |
  4 | Marta   | Herrera   | Gil       |     0.14 |    |       |       |            |              |    |        |           |           |        |
~~~
6. ¿Se podrían realizar las consultas anteriores con `NATURAL LEFT JOIN` o `NATURAL RIGHT JOIN`? Justifique su respuesta.
No se puede

# **Consultas resumen**

1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT SUM(total) AS total_sumatoria
  FROM pedido;

  total_sumatoria
--------------------
 20992.829999999998
~~~
2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT AVG(total) AS media_total
  FROM pedido;

    media_total
--------------------
 1312.0518749999999
~~~
3. Calcula el número total de comerciales distintos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT COUNT(DISTINCT cc.id)
  FROM pedido p
  LEFT JOIN comercial cc ON p.id_comercial = cc.id
  WHERE cc.id IS NOT NULL;

 count
-------
     6
~~~
4. Calcula el número total de clientes que aparecen en la tabla `cliente`.
~~~SQL
  SELECT COUNT(id)
  FROM cliente;

 count
-------
    10
~~~
5. Calcula cuál es la mayor cantidad que aparece en la tabla `pedido`.
~~~SQL
  SELECT MAX(total)
  FROM pedido;

 max
------
 5760
~~~
6. Calcula cuál es la menor cantidad que aparece en la tabla `pedido`.
~~~SQL
  SELECT MIN(total)
  FROM pedido;

  min
-------
 65.26
~~~
7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla `cliente`.
~~~SQL
  SELECT ciudad, MAX(categoria)
  FROM cliente
  GROUP BY ciudad;

 ciudad  | max
---------+-----
 AlmerÝa | 200
 Huelva  | 200
 Sevilla | 300
 Granada | 225
 JaÚn    | 300
 Cßdiz   | 100
~~~
8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidades el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos, la fecha y el valor de la cantidad.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, p.fecha, MAX(p.total) AS cantidad_maxima_dia
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  GROUP BY c.id, p.fecha;

 id | nombre | apellido1 | apellido2 |   fecha    | cantidad_maxima_dia
----+--------+-----------+-----------+------------+---------------------
  6 | MarÝa  | Santana   | Moreno    | 2017-02-02 |              145.82
  8 | Pepe   | Ruiz      | Santana   | 2015-06-27 |              250.45
  1 | Aar¾n  | Rivero    | G¾mez     | 2019-03-11 |             2389.23
  2 | Adela  | Salas     | DÝaz      | 2015-09-10 |                5760
  8 | Pepe   | Ruiz      | Santana   | 2016-10-10 |              2480.4
  6 | MarÝa  | Santana   | Moreno    | 2019-01-25 |              545.75
  3 | Adolfo | Rubio     | Flores    | 2016-08-17 |               75.29
  2 | Adela  | Salas     | DÝaz      | 2017-04-25 |              3045.6
  8 | Pepe   | Ruiz      | Santana   | 2016-08-17 |               110.5
  5 | Marcos | Loyola    | MÚndez    | 2017-10-05 |               150.5
  5 | Marcos | Loyola    | MÚndez    | 2017-09-10 |               948.5
  7 | Pilar  | Ruiz      |           | 2016-07-27 |              2400.6
  1 | Aar¾n  | Rivero    | G¾mez     | 2016-09-10 |              270.65
  4 | Adrißn | Sußrez    |           | 2017-10-10 |             1983.43
  2 | Adela  | Salas     | DÝaz      | 2017-10-05 |               65.26
~~~
9. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, p.fecha, MAX(p.total) AS cantidad_maxima_dia
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  WHERE p.total > 2000
  GROUP BY c.id, p.fecha;

 id | nombre | apellido1 | apellido2 |   fecha    | cantidad_maxima_dia
----+--------+-----------+-----------+------------+---------------------
  2 | Adela  | Salas     | DÝaz      | 2017-04-25 |              3045.6
  7 | Pilar  | Ruiz      |           | 2016-07-27 |              2400.6
  1 | Aar¾n  | Rivero    | G¾mez     | 2019-03-11 |             2389.23
  2 | Adela  | Salas     | DÝaz      | 2015-09-10 |                5760
  8 | Pepe   | Ruiz      | Santana   | 2016-10-10 |              2480.4
~~~
10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha `2016-08-17`. Muestra el identificador del comercial, nombre, apellidos y total.
~~~SQL
  SELECT cc.id, cc.nombre, cc.apellido1, cc.apellido2, MAX(p.total), p.fecha
  FROM comercial cc
  INNER JOIN pedido p ON cc.id = p.id_cliente
  WHERE p.fecha = '2016-08-17'
  GROUP BY cc.id , p.fecha;

 id | nombre  | apellido1 | apellido2 |  max  |   fecha
----+---------+-----------+-----------+-------+------------
  3 | Diego   | Flores    | Salas     | 75.29 | 2016-08-17
  8 | Alfredo | Ruiz      | Flores    | 110.5 | 2016-08-17
~~~
11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer en el listado indicando que el número de pedidos realizados es `0`.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, COUNT(p.id_cliente)
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  GROUP BY c.id
  ORDER BY c.id;

 id |  nombre   | apellido1 | apellido2 | count
----+-----------+-----------+-----------+-------
  1 | Aar¾n     | Rivero    | G¾mez     |     3
  2 | Adela     | Salas     | DÝaz      |     3
  3 | Adolfo    | Rubio     | Flores    |     1
  4 | Adrißn    | Sußrez    |           |     1
  5 | Marcos    | Loyola    | MÚndez    |     2
  6 | MarÝa     | Santana   | Moreno    |     2
  7 | Pilar     | Ruiz      |           |     1
  8 | Pepe      | Ruiz      | Santana   |     3
  9 | Guillermo | L¾pez     | G¾mez     |     0
 10 | Daniel    | Santana   | Loyola    |     0
~~~
12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes **durante el año 2017**.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, COUNT(p.id_cliente)
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  WHERE p.fecha > '2016-12-31' AND p.fecha < '2018-01-01'
  GROUP BY c.id
  ORDER BY c.id;

 id | nombre | apellido1 | apellido2 | count
----+--------+-----------+-----------+-------
  2 | Adela  | Salas     | DÝaz      |     2
  4 | Adrißn | Sußrez    |           |     1
  5 | Marcos | Loyola    | MÚndez    |     2
  6 | MarÝa  | Santana   | Moreno    |     1
~~~
13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es `0`. Puede hacer uso de la función `[IFNULL]`.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, COALESCE(MAX(p.total), 0)
  FROM cliente c
  LEFT JOIN pedido p ON c.id = p.id_cliente
  GROUP BY c.id, c.nombre, c.apellido1;

 id |  nombre   | apellido1 | coalesce
----+-----------+-----------+----------
  4 | Adrißn    | Sußrez    |  1983.43
 10 | Daniel    | Santana   |        0
  6 | MarÝa     | Santana   |   545.75
  2 | Adela     | Salas     |     5760
  9 | Guillermo | L¾pez     |        0
  7 | Pilar     | Ruiz      |   2400.6
  3 | Adolfo    | Rubio     |    75.29
  1 | Aar¾n     | Rivero    |  2389.23
  5 | Marcos    | Loyola    |    948.5
  8 | Pepe      | Ruiz      |   2480.4
~~~
14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.
~~~SQL
  SELECT MAX(total), EXTRACT(YEAR FROM fecha) AS año_agrupar
  FROM pedido
  GROUP BY año_agrupar;

   max   | año_agrupar
---------+-------------
  3045.6 |        2017
  2480.4 |        2016
 2389.23 |        2019
    5760 |        2015
~~~
15. Devuelve el número total de pedidos que se han realizado cada año.
~~~SQL
  SELECT COUNT(total), EXTRACT(YEAR FROM fecha) AS año_agrupar
  FROM pedido
  GROUP BY año_agrupar;

 count | año_agrupar
-------+-------------
     6 |        2017
     5 |        2016
     3 |        2019
     2 |        2015
~~~

# **Con operadores básicos de comparación**

1. Devuelve un listado con todos los pedidos que ha realizado `Adela Salas Díaz`. (Sin utilizar `INNER JOIN`).
~~~SQL
  SELECT *
  FROM pedido
  WHERE id_cliente = 2;

 id | total  |   fecha    | id_cliente | id_comercial
----+--------+------------+------------+--------------
  3 |  65.26 | 2017-10-05 |          2 |            1
  7 |   5760 | 2015-09-10 |          2 |            1
 12 | 3045.6 | 2017-04-25 |          2 |            1
~~~
2. Devuelve el número de pedidos en los que ha participado el comercial `Daniel Sáez Vega`. (Sin utilizar `INNER JOIN`)
~~~SQL
  SELECT *
  FROM pedido
  WHERE id_comercial = 1;

 id | total  |   fecha    | id_cliente | id_comercial
----+--------+------------+------------+--------------
  3 |  65.26 | 2017-10-05 |          2 |            1
  6 | 2400.6 | 2016-07-27 |          7 |            1
  7 |   5760 | 2015-09-10 |          2 |            1
 12 | 3045.6 | 2017-04-25 |          2 |            1
 13 | 545.75 | 2019-01-25 |          6 |            1
 14 | 145.82 | 2017-02-02 |          6 |            1
~~~
3. Devuelve los datos del cliente que realizó el pedido más caro en el año `2019`. (Sin utilizar `INNER JOIN`)
~~~SQL
  SELECT * 
  FROM cliente
  WHERE id = (SELECT id_cliente
  			FROM pedido
  			WHERE total = (SELECT MAX(total)
  						   FROM pedido
  						   WHERE fecha > '2018-12-31' AND fecha < '2020-01-01'));

 id | nombre | apellido1 | apellido2 | ciudad  | categoria
----+--------+-----------+-----------+---------+-----------
  1 | Aar¾n  | Rivero    | G¾mez     | AlmerÝa |       100
~~~
4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente `Pepe Ruiz Santana`.
~~~SQL
  SELECT fecha, total
  FROM pedido
  WHERE id_cliente = (SELECT id FROM cliente WHERE id = 8)
  ORDER BY total
  LIMIT 1;

   fecha    | total
------------+-------
 2016-08-17 | 110.5
~~~
5. Devuelve un listado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año `2017` con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.
~~~SQL

~~~
# **Subconsultas con `IN` y `NOT IN`**

1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).
~~~sql
    SELECT id, nombre
    FROM cliente
    WHERE id NOT IN (SELECT id_cliente FROM pedido);

 id |  nombre
----+-----------
  9 | Guillermo
 10 | Daniel
~~~
2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).
~~~sql
  SELECT id, nombre
  FROM comercial
  WHERE id NOT IN (SELECT id_comercial FROM pedido);

 id | nombre
----+---------
  4 | Marta
  8 | Alfredo
~~~

# **Subconsultas con `EXISTS` y `NOT EXISTS`**

1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).
~~~SQL
  SELECT *
  FROM cliente c
  WHERE NOT EXISTS (SELECT id_cliente FROM pedido p WHERE p.id_cliente = c.id);

 id |  nombre   | apellido1 | apellido2 | ciudad  | categoria
----+-----------+-----------+-----------+---------+-----------
 10 | Daniel    | Santana   | Loyola    | Sevilla |       125
  9 | Guillermo | L¾pez     | G¾mez     | Granada |       225
~~~
2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).
~~~SQL
  SELECT *
  FROM comercial cc
  WHERE NOT EXISTS (SELECT id_comercial FROM pedido p WHERE p.id_comercial = cc.id);

 id | nombre  | apellido1 | apellido2 | comisi¾n
----+---------+-----------+-----------+----------
  8 | Alfredo | Ruiz      | Flores    |     0.05
  4 | Marta   | Herrera   | Gil       |     0.14
~~~
