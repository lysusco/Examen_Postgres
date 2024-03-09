# Examen_Postgres

# **Consultas sobre una tabla**

1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.
~~~SQL
  SELECT *
  FROM pedido
  ORDER BY fecha DESC;
~~~
2. Devuelve todos los datos de los dos pedidos de mayor valor.
~~~SQL
  SELECT *
  FROM pedido
  ORDER BY total DESC
  LIMIT 2;
~~~
3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.
~~~SQL
  SELECT DISTINCT id_cliente
  FROM pedido;
~~~
4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.
~~~SQL
  SELECT *
  FROM pedido
  WHERE total > 500 AND fecha < '2018-01-01' AND fecha > '2016-12-31';
~~~
5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.
~~~SQL
  SELECT nombre, apellido1, apellido2
  FROM comercial
  WHERE comisión BETWEEN 0.05 AND 0.11;
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
