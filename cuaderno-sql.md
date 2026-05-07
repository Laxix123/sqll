-- 26-03-2026

-- Mostrar todas las personas registradas
SELECT	*
FROM	PERSONA

SELECT	rut_persona, nombre_persona, id_estado_civil
FROM	Persona

-- Mostrar todas las personas y su estado civil
SELECT	per.nombre_persona, e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil

-- Mostrar las personas solteras
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.nombre_estado_civil	= 'Soltero'

-- Mostrar las personas casadas y viudas
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	(e_c.nombre_estado_civil	= 'Casado' OR e_c.nombre_estado_civil	= 'Viudo')

-- Mostrar las personas casadas y viudas (IN)
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28LIKE_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue

SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.nombre_estado_civil	IN ('Casado', 'Viudo')

-- Mostrar las personas que tengan id_estado_civil entre 1 y 3
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil, e_c.id_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.id_estado_civil		>= 1 AND	e_c.id_estado_civil		<= 3

-- BETWEEN, LIKE, NOT IN

-- Mostrar las personas que tengan id_estado_civil entre 1 y 3 (BETWEEN)
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/between-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28BETWEEN_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil, e_c.id_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.id_estado_civil		BETWEEN 1 AND 3

-- LIKE
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28LIKE_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue

-- Mostrar las personas con nombre que comienzan con la letra 'C'
SELECT	per.nombre_persona
FROM	PERSONA per
WHERE	per.nombre_persona	LIKE 'c%'

-- Mostrar las personas con nombre que contiene la letra 'o'
SELECT	per.nombre_persona
FROM	PERSONA per
WHERE	per.nombre_persona	LIKE '%o%'

-- 09-04-2026
-- BD VIDEOCLUB

-- Mostrar las películas existentes en el VC y su categoría
SELECT	pel.nombre_pelicula, cat. nombre_categoria
FROM	PELICULA pel, CATEGORIA cat
WHERE	pel.id_categoria	= cat.id_categoria

-- Determinar la cantidad de películas existentes en mi VC
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/count-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28COUNT_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue
SELECT	COUNT(*)
FROM	PELICULA

/* Determinar la cantidad de películas existentes por categoría, 
ordenado descendentemente por categoría */
SELECT	cat. nombre_categoria, COUNT(*)
FROM	PELICULA pel, CATEGORIA cat
WHERE	pel.id_categoria	= cat.id_categoria
GROUP BY cat. nombre_categoria
ORDER BY cat. nombre_categoria DESC

/* Determinar la cantidad de películas existentes por género, 
ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	PELICULA pel, GENERO_PELICULA g_p, GENERO gen
WHERE	pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
GROUP BY gen.nombre_genero
ORDER BY gen.nombre_genero ASC

-- Determinar la fecha de arriendo de la última película arrendada
SELECT	MAX(pre.fecha_prestamo)
FROM	PRESTAMO pre, COPIA_PELICULA c_p, PELICULA pel
WHERE	pre.id_pelicula		= c_p.id_pelicula
	AND	pre.numero_copia	= c_p.numero_copia
	AND	c_p.id_pelicula		= pel.id_pelicula

-- Determinar el nombre y fecha de arriendo de la última película arrendada, por película
SELECT	pel.nombre_pelicula, MAX(pre.fecha_prestamo)
FROM	PRESTAMO pre, COPIA_PELICULA c_p, PELICULA pel
WHERE	pre.id_pelicula		= c_p.id_pelicula
	AND	pre.numero_copia	= c_p.numero_copia
	AND	c_p.id_pelicula		= pel.id_pelicula
GROUP BY pel.nombre_pelicula

-- Determinar el nombre y fecha de arriendo de la última película arrendada (Tarea)

-- 10-04-2026
-- Determinar el monto promedio de multas aplicadas, por categoría
SELECT	cat.nombre_categoria, AVG(pre.monto_multa)
FROM	CATEGORIA cat, PELICULA pel, COPIA_PELICULA c_p, PRESTAMO pre
WHERE	cat.id_categoria	= pel.id_categoria
	AND	pel.id_pelicula		= c_p.id_pelicula
	AND	c_p.id_pelicula		= pre.id_pelicula
	AND	c_p.numero_copia	= pre.numero_copia
GROUP BY cat.nombre_categoria

/* Determinar la cantidad de películas existentes por género, de aquellos géneros
con más de 2 películas, ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	PELICULA pel, GENERO_PELICULA g_p, GENERO gen
WHERE	pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
GROUP BY gen.nombre_genero
HAVING	COUNT(*) > 2
ORDER BY gen.nombre_genero ASC

-- 16-04-2026

/* Determinar la cantidad de películas, 'TODO ESPECTADOR' ARRENDADAS Y DEVUELTAS por género, de aquellos géneros
con más de 2 películas, ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	CATEGORIA cat, PELICULA pel, GENERO_PELICULA g_p, GENERO gen,
		COPIA_PELICULA c_p, PRESTAMO pre
WHERE	cat.id_categoria	= pel.id_categoria
	AND pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
	AND	pel.id_pelicula		= c_p.id_pelicula
	AND	c_p.id_pelicula		= pre.id_pelicula
	AND	c_p.numero_copia	= pre.numero_copia
	AND	cat.nombre_categoria	= 'Todo Espectador'
	AND	pre.fecha_entrega		IS NOT NULL
GROUP BY gen.nombre_genero
HAVING	COUNT(*) > 2
ORDER BY gen.nombre_genero ASC

-
--FUNCIONES DE CADENA O STRING
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/ascii-transact-sql?view=sql-server-ver17

SELECT 'Patricio Salgado                                           '
SELECT TRIM('Patricio Salgado                                           ')

-- Mostrar los clientes del VC, y la cantidad de caracteres que tiene su nombre
SELECT	cli.nombre_cliente, LEN(cli.nombre_cliente)
FROM	CLIENTE cli

-- Mostrar los clientes cuyo nombre termina con la letra 'a'
SELECT	cli.nombre_cliente
FROM	CLIENTE cli
WHERE	RTRIM(cli.nombre_cliente)	like '%a'

/*  Mostrar los clientes que en su nombre contienen la letra 'Y', 
y reemplazarla por 'LL' */
SELECT	cli.nombre_cliente, REPLACE(cli.nombre_cliente, 'y', 'll')
FROM	CLIENTE cli
WHERE	RTRIM(cli.nombre_cliente)	like '%y%'

-- 17-04-2026
-- PRUEBA 1

-- 23-04-2026

-- Mostrar el segundo nombre del cliente con rut '3-k'

/* Tarea: A partir del nombre y la dirección del cliente, generar el correo institucional (ucsh.cl). 
Ej: Condoritocasa1@ucsh.cl */

-- FUNCIONES MATEMÁTICAS
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/abs-transact-sql?view=sql-server-ver17

-- Determinar el área de un circunferencia de radio 3
SELECT	PI()*3*3
SELECT	PI()*POWER(3, 2)
SELECT	PI()*SQUARE(3)

-- Determinar el largo de la hipotenusa de un triangulo rectángulo de lados 3 y 4.

SELECT	SQRT(SQUARE(3) + SQUARE(4))
SELECT	POWER(SQUARE(3) + SQUARE(4), 0.5)

-- Determinar el valor del iva a pagar de un artículo con precio neto de $2471
SELECT ROUND(2471*0.19, 0)

-- 24-04-2026

-- Tarea: Determinar el ajuste de sencillo para un pago en efectivo de $2003
SELECT ROUND(2008, -1)

--FUNCIONES FECHA
-- Determinar la fecha y hora actual
SELECT SYSDATETIME(), GETDATE()

--Determinar el número del día
SELECT DATEPART(day, getdate())
SELECT DAY(GETDATE())

-- Cómo determinar el nombre del día (2 FORMAS)
SELECT	DATENAME(DW, GETDATE())

-- 30-04-2026
--Determinar la fecha que será en 47 días más

SELECT DATEADD(day,47,GETDATE())

--Determinar que fue hace 23 días

SELECT DATEADD(day,-23,GETDATE())

--Determinar qué fecha será en 3 meses

SELECT DATEADD(MONTH,3,GETDATE())

--Determinar la edad aproximada (en años) de una persona que nacio el 14-05-1999

SELECT DATEDIFF(YEAR,'1999-05-14',GETDATE())

SELECT DATEDIFF(YY,GETDATE(),'1999-05-14')

SELECT DATEDIFF(YYYY,GETDATE(),'1999-05-14')

SELECT DATEDIFF(DD,'1999-05-14',GETDATE())/ 365

SELECT year(GETDATE()) - year(('1999-05-14'))

--Determinar si la fecha '18-09-2026' es una fecha real

SELECT ISDATE ( '18-09.2026' )

--Determinar la edad aproximada en años, de cada uno de los clientes del VC

SELECT cli.nombre_cliente,DATEDIFF(YEAR,cli.fecha_nacimiento_cliente,GETDATE())
FROM CLIENTE cli

-- Determinar la cantidad de películas entregadas atrasadas 

SELECT  COUNT(*) 'cantidad'
FROM PELICULA pl , COPIA_PELICULA cp , PRESTAMO pr
WHERE pl.id_pelicula = cp.id_pelicula
 AND  cp.id_pelicula = pr.id_pelicula
 AND  cp.numero_copia = pr.numero_copia
 AND	pr.fecha_entrega > pr.fecha_devolucion


-- Determinar la cantidad de días de atraso que tienen las películas no entregadas, identificando el cliente y la película 

SELECT cli.nombre_cliente , pl.nombre_pelicula ,COUNT(*) 'cantidad'
FROM    PELICULA pl , COPIA_PELICULA cp ,PRESTAMO pr, CLIENTE cli
WHERE  cli.rut_cliente = pr.rut_cliente
 AND   pr.id_pelicula = cp.id_pelicula
 AND   pr.numero_copia = cp.numero_copia
 AND   cp.id_pelicula = pl.id_pelicula
 AND   pr.fecha_entrega is null
 GROUP BY cli.nombre_cliente , pl.nombre_pelicula 


-- Determinar el monto a pagar de lso arriendos vigentes atrasados, identificando el cliente y la pelicula



SELECT cl.nombre_cliente, REPLACE(cl.nombre_cliente,'y','LL')
FROM CLIENTE cl
WHERE TRIM(cl.nombre_cliente) LIKE '%Y%' 
 

=======================================================================
=======================================================================
=======================================================================

-- 26-03-2026

-- Mostrar todas las personas registradas
SELECT	*
FROM	PERSONA

SELECT	rut_persona, nombre_persona, id_estado_civil
FROM	Persona

-- Mostrar todas las personas y su estado civil
SELECT	per.nombre_persona, e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil

-- Mostrar las personas solteras
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.nombre_estado_civil	= 'Soltero'

-- Mostrar las personas casadas y viudas
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	(e_c.nombre_estado_civil	= 'Casado' OR e_c.nombre_estado_civil	= 'Viudo')

-- Mostrar las personas casadas y viudas (IN)
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28LIKE_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue

SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.nombre_estado_civil	IN ('Casado', 'Viudo')

-- Mostrar las personas que tengan id_estado_civil entre 1 y 3
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil, e_c.id_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.id_estado_civil		>= 1 AND	e_c.id_estado_civil		<= 3

-- BETWEEN, LIKE, NOT IN

-- Mostrar las personas que tengan id_estado_civil entre 1 y 3 (BETWEEN)
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/between-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28BETWEEN_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue
SELECT	per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil, e_c.id_estado_civil
FROM	PERSONA per, Estado_Civil e_c
WHERE	per.id_estado_civil		= e_c.id_estado_civil
	AND	e_c.id_estado_civil		BETWEEN 1 AND 3

-- LIKE
-- https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28LIKE_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue

-- Mostrar las personas con nombre que comienzan con la letra 'C'
SELECT	per.nombre_persona
FROM	PERSONA per
WHERE	per.nombre_persona	LIKE 'c%'

-- Mostrar las personas con nombre que contiene la letra 'o'
SELECT	per.nombre_persona
FROM	PERSONA per
WHERE	per.nombre_persona	LIKE '%o%'

-- 09-04-2026
-- BD VIDEOCLUB

-- Mostrar las películas existentes en el VC y su categoría
SELECT	pel.nombre_pelicula, cat. nombre_categoria
FROM	PELICULA pel, CATEGORIA cat
WHERE	pel.id_categoria	= cat.id_categoria

-- Determinar la cantidad de películas existentes en mi VC
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/count-transact-sql?view=sql-server-ver17&f1url=%3FappId%3DDev15IDEF1%26l%3DEN-US%26k%3Dk%28COUNT_TSQL%29%3Bk%28sql13.swb.tsqlresults.f1%29%3Bk%28sql13.swb.tsqlquery.f1%29%3Bk%28MiscellaneousFilesProject%29%3Bk%28DevLang-TSQL%29%26rd%3Dtrue
SELECT	COUNT(*)
FROM	PELICULA

/* Determinar la cantidad de películas existentes por categoría, 
ordenado descendentemente por categoría */
SELECT	cat. nombre_categoria, COUNT(*)
FROM	PELICULA pel, CATEGORIA cat
WHERE	pel.id_categoria	= cat.id_categoria
GROUP BY cat. nombre_categoria
ORDER BY cat. nombre_categoria DESC

/* Determinar la cantidad de películas existentes por género, 
ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	PELICULA pel, GENERO_PELICULA g_p, GENERO gen
WHERE	pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
GROUP BY gen.nombre_genero
ORDER BY gen.nombre_genero ASC

-- Determinar la fecha de arriendo de la última película arrendada
SELECT	MAX(pre.fecha_prestamo)
FROM	PRESTAMO pre, COPIA_PELICULA c_p, PELICULA pel
WHERE	pre.id_pelicula		= c_p.id_pelicula
	AND	pre.numero_copia	= c_p.numero_copia
	AND	c_p.id_pelicula		= pel.id_pelicula

-- Determinar el nombre y fecha de arriendo de la última película arrendada, por película
SELECT	pel.nombre_pelicula, MAX(pre.fecha_prestamo)
FROM	PRESTAMO pre, COPIA_PELICULA c_p, PELICULA pel
WHERE	pre.id_pelicula		= c_p.id_pelicula
	AND	pre.numero_copia	= c_p.numero_copia
	AND	c_p.id_pelicula		= pel.id_pelicula
GROUP BY pel.nombre_pelicula

-- Determinar el nombre y fecha de arriendo de la última película arrendada (Tarea)

-- 10-04-2026
-- Determinar el monto promedio de multas aplicadas, por categoría
SELECT	cat.nombre_categoria, AVG(pre.monto_multa)
FROM	CATEGORIA cat, PELICULA pel, COPIA_PELICULA c_p, PRESTAMO pre
WHERE	cat.id_categoria	= pel.id_categoria
	AND	pel.id_pelicula		= c_p.id_pelicula
	AND	c_p.id_pelicula		= pre.id_pelicula
	AND	c_p.numero_copia	= pre.numero_copia
GROUP BY cat.nombre_categoria

/* Determinar la cantidad de películas existentes por género, de aquellos géneros
con más de 2 películas, ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	PELICULA pel, GENERO_PELICULA g_p, GENERO gen
WHERE	pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
GROUP BY gen.nombre_genero
HAVING	COUNT(*) > 2
ORDER BY gen.nombre_genero ASC

-- 16-04-2026

/* Determinar la cantidad de películas, 'TODO ESPECTADOR' ARRENDADAS Y DEVUELTAS por género, de aquellos géneros
con más de 2 películas, ordenado ascendentemente por género */
SELECT	gen.nombre_genero, COUNT(*)
FROM	CATEGORIA cat, PELICULA pel, GENERO_PELICULA g_p, GENERO gen,
		COPIA_PELICULA c_p, PRESTAMO pre
WHERE	cat.id_categoria	= pel.id_categoria
	AND pel.id_pelicula		= g_p.id_pelicula
	AND	g_p.id_genero		= gen.id_genero
	AND	pel.id_pelicula		= c_p.id_pelicula
	AND	c_p.id_pelicula		= pre.id_pelicula
	AND	c_p.numero_copia	= pre.numero_copia
	AND	cat.nombre_categoria	= 'Todo Espectador'
	AND	pre.fecha_entrega		IS NOT NULL
GROUP BY gen.nombre_genero
HAVING	COUNT(*) > 2
ORDER BY gen.nombre_genero ASC

-
--FUNCIONES DE CADENA O STRING
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/ascii-transact-sql?view=sql-server-ver17

SELECT 'Patricio Salgado                                           '
SELECT TRIM('Patricio Salgado                                           ')

-- Mostrar los clientes del VC, y la cantidad de caracteres que tiene su nombre
SELECT	cli.nombre_cliente, LEN(cli.nombre_cliente)
FROM	CLIENTE cli

-- Mostrar los clientes cuyo nombre termina con la letra 'a'
SELECT	cli.nombre_cliente
FROM	CLIENTE cli
WHERE	RTRIM(cli.nombre_cliente)	like '%a'

/*  Mostrar los clientes que en su nombre contienen la letra 'Y', 
y reemplazarla por 'LL' */
SELECT	cli.nombre_cliente, REPLACE(cli.nombre_cliente, 'y', 'll')
FROM	CLIENTE cli
WHERE	RTRIM(cli.nombre_cliente)	like '%y%'

-- 17-04-2026
-- PRUEBA 1

-- 23-04-2026

-- Mostrar el segundo nombre del cliente con rut '3-k'

/* Tarea: A partir del nombre y la dirección del cliente, generar el correo institucional (ucsh.cl). 
Ej: Condoritocasa1@ucsh.cl */

-- FUNCIONES MATEMÁTICAS
-- https://learn.microsoft.com/en-us/sql/t-sql/functions/abs-transact-sql?view=sql-server-ver17

-- Determinar el área de un circunferencia de radio 3
SELECT	PI()*3*3
SELECT	PI()*POWER(3, 2)
SELECT	PI()*SQUARE(3)

-- Determinar el largo de la hipotenusa de un triangulo rectángulo de lados 3 y 4.

SELECT	SQRT(SQUARE(3) + SQUARE(4))
SELECT	POWER(SQUARE(3) + SQUARE(4), 0.5)

-- Determinar el valor del iva a pagar de un artículo con precio neto de $2471
SELECT ROUND(2471*0.19, 0)

-- 24-04-2026

-- Tarea: Determinar el ajuste de sencillo para un pago en efectivo de $2003
SELECT ROUND(2008, -1)

--FUNCIONES FECHA
-- Determinar la fecha y hora actual
SELECT SYSDATETIME(), GETDATE()

--Determinar el número del día
SELECT DATEPART(day, getdate())
SELECT DAY(GETDATE())

-- Cómo determinar el nombre del día (2 FORMAS)
SELECT	DATENAME(DW, GETDATE())

-- 30-04-2026
--Determinar la fecha que será en 47 días más

/* Determinar el monto a pagar de la multa por arriendos vigentes atrasados,
identificando el cliente y la película */
SELECT cli.nombre_cliente,pel.nombre_pelicula, c_p.numero_copia,
pel.arriendo_diario * DATEDIFF(DAY, pre.fecha_devolucion, GETDATE())
FROM PELICULA pel, COPIA_PELICULA c_p, PRESTAMO pre, CLIENTE cli
WHERE pel.id_pelicula = c_p.id_pelicula
AND pre.id_pelicula = c_p.id_pelicula
AND pre.numero_copia = c_p.numero_copia
AND pre.rut_cliente = cli.rut_cliente
AND pre.fecha_entrega IS NULL
AND pre.fecha_devolucion < GETDATE()

/* determinar la cantidad de peliculas arrendadas y entregadas por clientes,
en el mes de cumpleaños, de la comuna de Independencia y Santiago */
SELECT cli.nombre_cliente, COUNT (*) 'cantidad'
FROM COMUNA com, CLIENTE cli, PRESTAMO pre
WHERE com.id_comuna = cli.id_comuna
AND Cli.rut_cliente = pre.rut_cliente
AND MONTH(cli.fecha_nacimiento_cliente) = MONTH(pre.fecha_prestamo)
AND com.nombre_comuna IN ('Independencia' , 'Santiago')
GROUP BY cli.nombre_cliente



/* Determinar los clientes que tienen películas atrasadas, de género de Anime o Suspenso,
arrendadas durante el segundo o cuarto trimestre, con un nombre de película no superior a 10 letras */


SELECT cli.nombre_cliente  
FROM GENERO gen , GENERO_PELICULA g_p, PELICULA pel, 
     COPIA_PELICULA c_p, PRESTAMO pre , CLIENTE cli
WHERE gen.id_genero = g_p.id_genero
  and g_p.id_pelicula = pel.id_pelicula
  and pel.id_pelicula = c_p.id_pelicula
  and c_p.id_pelicula = pre.id_pelicula
  and c_p.numero_copia = pre.numero_copia
  and pre.rut_cliente = cli.rut_cliente
  and pre.fecha_entrega IS NULL
  and pre.fecha_devolucion < GETDATE()
  and gen.nombre_genero IN ('Anime','Suspenso')
  and LEN( trim(pel.nombre_pelicula)) <= 10
  and DATEPART(QUARTER, pre.fecha_prestamo)   IN (2 ,4)
  and MONTH(pre.fecha_prestamo)               IN (4,5,6,10,11,12)  

  -- Determinar el estado (Oportuno o Atrasado) de los arriendos ya devueltos 

SELECT pre.id_pelicula , pre.numero_copia , pre.rut_cliente,
	CASE 
	WHEN pre.fecha_entrega <= pre.fecha_devolucion THEN 'Entrega Oportuna'
	ELSE 'Entrega Atrasada'
	END AS 'ESTADO'
FROM PRESTAMO pre
WHERE pre.fecha_entrega IS NOT NULL

--Determinar el estado (Adelantado, Al día o Atrasado) de los arriendos ya devueltos 

SELECT pre.id_pelicula , pre.numero_copia , pre.rut_cliente,
	CASE 
	WHEN pre.fecha_entrega < pre.fecha_devolucion THEN 'ADELANTADO' 
	WHEN pre.fecha_entrega = pre.fecha_devolucion THEN 'AL DÍA'
	ELSE 'ATRASADO'
	END AS 'ESTADO'
FROM PRESTAMO pre
WHERE pre.fecha_entrega IS NOT NULL




