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

![image](https://github.com/lysusco/Examen_Postgres/assets/127136702/caed90d4-b29d-40c7-a3a1-ea65c2df4d73)
~~~
6. Devuelve el valor de la comisión de mayor valor que existe en la tabla `comercial`.
~~~SQL
  SELECT MAX(comisión)
  FROM comercial;
~~~
7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido **no** es `NULL`. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.
~~~SQL
  SELECT id, nombre, apellido1, apellido2
  FROM cliente
  WHERE apellido2 IS NOT NULL
  ORDER BY apellido1, apellido2, nombre;
~~~
8. Devuelve un listado de los nombres de los clientes que empiezan por `A` y terminan por `n` y también los nombres que empiezan por `P`. El listado deberá estar ordenado alfabéticamente.
~~~SQL
  SELECT nombre
  FROM cliente
  WHERE nombre LIKE 'A%n' OR nombre LIKE 'P%'
  ORDER BY nombre;
~~~
9. Devuelve un listado de los nombres de los clientes que **no** empiezan por `A`. El listado deberá estar ordenado alfabéticamente.
~~~SQL
  SELECT nombre
  FROM cliente
  WHERE nombre NOT LIKE 'A%'
  ORDER BY nombre;
~~~
10. Devuelve un listado con los nombres de los comerciales que terminan por `el` o `o`. Tenga en cuenta que se deberán eliminar los nombres repetidos.
~~~SQL
  SELECT DISTINCT nombre
  FROM comercial
  WHERE nombre LIKE '%el' OR nombre LIKE '%o';
~~~

# **Consultas multitabla (Composición interna)**

Resuelva todas las consultas utilizando la sintaxis de `SQL1` y `SQL2`.

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.
~~~SQL
  SELECT DISTINCT c.id, c.nombre, c.apellido1, c.apellido2
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id;
~~~
2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  ORDER BY c.nombre;
~~~
3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.
~~~SQL
  SELECT *
  FROM pedido p
  INNER JOIN comercial c ON c.id = p.id_comercial
  ORDER BY c.nombre;
~~~
4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON c.id = p.id_cliente
  INNER JOIN comercial cc ON cc.id = p.id_comercial;
~~~
5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año `2017`, cuya cantidad esté entre `300` € y `1000` €.
~~~SQL
  SELECT *
  FROM cliente c
  INNER JOIN pedido p ON c.id = p.id_cliente
  WHERE p.total BETWEEN 300 AND 1000 AND p.fecha > '2016-12-31' AND p.fecha < '2018-01-01';
~~~
6. Devuelve el nombre y los apellidos de todos los comerciales que ha participado en algún pedido realizado por `María Santana Moreno`.
~~~SQL
  SELECT DISTINCT cc.nombre, cc.apellido1, cc.apellido2
  FROM comercial cc
  INNER JOIN pedido p ON p.id_comercial = cc.id
  INNER JOIN cliente c ON p.id_cliente = c.id
  WHERE c.nombre = 'María' AND c.apellido1 = 'Santana' AND c.apellido2 = 'Moreno';
~~~
7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial `Daniel Sáez Vega`.
~~~SQL
  SELECT DISTINCT c.nombre
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  INNER JOIN comercial cc ON p.id_comercial = cc.id
  WHERE cc.nombre = 'Daniel' AND cc.apellido1 = 'Sáez' AND cc.apellido2 = 'Vega';
~~~

# **Consultas multitabla (Composición externa)**

Resuelva todas las consultas utilizando las cláusulas `LEFT JOIN` y `RIGHT JOIN`.

1. Devuelve un listado con **todos los clientes** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.
~~~SQL
  SELECT *
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  ORDER BY c.apellido1, c.apellido2, c.nombre;
~~~
2. Devuelve un listado con **todos los comerciales** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  ORDER BY cc.apellido1, cc.apellido2, cc.nombre;
~~~
3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.
~~~SQL
  SELECT *
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  WHERE p.id_cliente IS NULL;
~~~
4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  WHERE p.id_comercial IS NULL;
~~~
5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.
~~~SQL
  SELECT *
  FROM comercial cc
  LEFT JOIN pedido p ON p.id_comercial = cc.id
  LEFT JOIN cliente c ON p.id_cliente = c.id
  WHERE p.id_comercial IS NULL OR p.id_cliente IS NULL;
~~~
6. ¿Se podrían realizar las consultas anteriores con `NATURAL LEFT JOIN` o `NATURAL RIGHT JOIN`? Justifique su respuesta.
No se puede

# **Consultas resumen**

1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT SUM(total) AS total_sumatoria
  FROM pedido;
~~~
2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT AVG(total) AS media_total
  FROM pedido;
~~~
3. Calcula el número total de comerciales distintos que aparecen en la tabla `pedido`.
~~~SQL
  SELECT COUNT(DISTINCT cc.id)
  FROM pedido p
  LEFT JOIN comercial cc ON p.id_comercial = cc.id
  WHERE cc.id IS NOT NULL;
~~~
4. Calcula el número total de clientes que aparecen en la tabla `cliente`.
~~~SQL
  SELECT COUNT(id)
  FROM cliente;
~~~
5. Calcula cuál es la mayor cantidad que aparece en la tabla `pedido`.
~~~SQL
  SELECT MAX(total)
  FROM pedido;
~~~
6. Calcula cuál es la menor cantidad que aparece en la tabla `pedido`.
~~~SQL
  SELECT MIN(total)
  FROM pedido;
~~~
7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla `cliente`.
~~~SQL
  SELECT ciudad, MAX(categoria)
  FROM cliente
  GROUP BY ciudad;
~~~
8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidades el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos, la fecha y el valor de la cantidad.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, p.fecha, MAX(p.total) AS cantidad_maxima_dia
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  GROUP BY c.id, p.fecha;
~~~
9. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, p.fecha, MAX(p.total) AS cantidad_maxima_dia
  FROM cliente c
  INNER JOIN pedido p ON p.id_cliente = c.id
  WHERE p.total > 2000
  GROUP BY c.id, p.fecha;
~~~
10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha `2016-08-17`. Muestra el identificador del comercial, nombre, apellidos y total.
~~~SQL
  SELECT cc.id, cc.nombre, cc.apellido1, cc.apellido2, MAX(p.total), p.fecha
  FROM comercial cc
  INNER JOIN pedido p ON cc.id = p.id_cliente
  WHERE p.fecha = '2016-08-17'
  GROUP BY cc.id , p.fecha;
~~~
11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer en el listado indicando que el número de pedidos realizados es `0`.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, COUNT(p.id_cliente)
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  GROUP BY c.id
  ORDER BY c.id;
~~~
12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes **durante el año 2017**.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, c.apellido2, COUNT(p.id_cliente)
  FROM cliente c
  LEFT JOIN pedido p ON p.id_cliente = c.id
  WHERE p.fecha > '2016-12-31' AND p.fecha < '2018-01-01'
  GROUP BY c.id
  ORDER BY c.id;
~~~
13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es `0`. Puede hacer uso de la función `[IFNULL]`.
~~~SQL
  SELECT c.id, c.nombre, c.apellido1, COALESCE(MAX(p.total), 0)
  FROM cliente c
  LEFT JOIN pedido p ON c.id = p.id_cliente
  GROUP BY c.id, c.nombre, c.apellido1;
~~~
14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.
~~~SQL
  SELECT MAX(total), EXTRACT(YEAR FROM fecha) AS año_agrupar
  FROM pedido
  GROUP BY año_agrupar;
~~~
15. Devuelve el número total de pedidos que se han realizado cada año.
~~~SQL
  SELECT COUNT(total), EXTRACT(YEAR FROM fecha) AS año_agrupar
  FROM pedido
  GROUP BY año_agrupar;
~~~
