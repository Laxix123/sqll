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


/* Determinar la cantidad de películas,'TODO ESPECTADOR' ARRENDADAS Y DEVUELTAS por género, de aquellos géneros
con más de 2 películas, ordenado ascendentemente por género */

SELECT	gen.nombre_genero, COUNT(*)
FROM	PELICULA pel, GENERO_PELICULA g_p, GENERO gen, CATEGORIA cat ,COPIA_PELICULA cop, PRESTAMO pre
WHERE	cat.id_categoria = pel.id_categoria
	AND pel.id_pelicula  = g_p.id_pelicula
	AND	g_p.id_genero    = gen.id_genero
	AND pel.id_pelicula  = cop.id_pelicula
	AND cop.id_pelicula  = pre.id_pelicula
	AND cop.numero_copia = pre.numero_copia
	AND cat.nombre_categoria = 'TODO ESPECTADOR'
	AND pre.fecha_entrega IS NOT NULL
GROUP BY gen.nombre_genero 
HAVING	COUNT(*) >= 2
ORDER BY gen.nombre_genero ASC



SELECT TRIM ('patricio salgado')

--mostrar los clientes del VC, y la cantidad de caracteres que tiene su nombre 

SELECT cl.nombre_cliente ,LEN( cl.nombre_cliente)
FROM CLIENTE cl

-- Mostrar los clientes cuyos nombres termina con la letra 'a'

SELECT cl.nombre_cliente
FROM CLIENTE cl
WHERE TRIM(cl.nombre_cliente) LIKE '%a'

/* Mostrar los clientes que en su nombre contienen la letra 'Y' , 
y reemplazarla por 'LL' */

SELECT cl.nombre_cliente, REPLACE(cl.nombre_cliente,'y','LL')
FROM CLIENTE cl
WHERE TRIM(cl.nombre_cliente) LIKE '%Y%' 
 


