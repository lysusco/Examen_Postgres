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

