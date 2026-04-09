-- 26-03-2026

-- Mostrar todas las personas registradas SELECT * FROM PERSONA

SELECT rut_persona, nombre_persona, id_estado_civil FROM Persona

-- Mostrar todas las personas y su estado civil 

SELECT per.nombre_persona, e_c.nombre_estado_civil 
FROM PERSONA per, Estado_Civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil

-- Mostrar las personas solteras 

SELECT per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil 
FROM PERSONA per, Estado_Civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil AND e_c.nombre_estado_civil = 'Soltero'

-- Mostrar las personas casadas y viudas 

SELECT per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil 
FROM PERSONA per, Estado_Civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil AND (e_c.nombre_estado_civil = 'Casado' OR e_c.nombre_estado_civil = 'Viudo')

SELECT per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil 
FROM PERSONA per, Estado_Civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil AND e_c.nombre_estado_civil IN ('Casado', 'Viudo')

-- Mostrar las personas que tengan id_estado_civil entre 1 y 3 
SELECT per.nombre_persona 'Persona Registrada', e_c.nombre_estado_civil, e_c.id_estado_civil
FROM PERSONA per, Estado_Civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil AND e_c.id_estado_civil >= 1 AND e_c.id_estado_civil <= 3

--27-03-2026 - between --https://learn.microsoft.com/en-us/sql/t-sql/language-elements/between-transact-sql?view=sql-server-ver17 
SELECT per.nombre_persona 'persona registrada', e_c.nombre_estado_civil, e_c.id_estado_civil 
FROM Persona per, Estado_civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil and e_c.id_estado_civil between 1 and 3

--mostrar las persona con su nombre que comienza con la letra 'c' - LIKE --https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver17 
SELECT per.nombre_persona 'Persona registrada' 
FROM Persona per 
WHERE per.nombre_persona LIKE 'c%'

--mostrar las personas que en su nombre tengan la letra 'i' - LIKE 
SELECT per.nombre_persona 'Persona registrada' 
FROM Persona per 
WHERE per.nombre_persona LIKE '%i%'

--mostrar las personas que su nombre termina con 'o' -LIKE 
SELECT per.nombre_persona 'Persona registrada' 
FROM Persona per 
WHERE per.nombre_persona LIKE '%o'

--mostrar las personas con estado civil diferente de casado y soltero 
SELECT per.nombre_persona 'Persona registrada', e_c.nombre_estado_civil 
FROM Persona per, Estado_civil e_c 
WHERE per.id_estado_civil = e_c.id_estado_civil and e_c.nombre_estado_civil not IN ('casado' , 'soltero')




--- Mostrar las peliculas existentes en el VC y su categoria

SELECT pel.nombre_pelicula, cat.nombre_categoria
FROM PELICULA pel, CATEGORIA cat
WHERE pel.id_categoria = cat.id_categoria

-- Determine la cantidad de peliculas existentes en mi VC
SELECT COUNT(*)
FROM PELICULA

/*Determinar la cantidad de películas existentes por categoría, 
ordenado descendentemente por categoría*/

SELECT cat.nombre_categoria,  COUNT(*)
FROM  PELICULA pel, CATEGORIA cat
WHERE pel.id_categoria = cat.id_categoria
GROUP BY cat.nombre_categoria 
ORDER BY cat.nombre_categoria DESC

/*Determinar la cantidad de películas existentes por género, 
ordenado Ascendentemente por género*/

SELECT gen.nombre_genero,  COUNT(*)
FROM  PELICULA pel, GENERO_PELICULA g_p, GENERO gen
WHERE pel.id_pelicula = g_p.id_pelicula
	AND g_p.id_genero = gen.id_genero
GROUP BY gen.nombre_genero
ORDER BY gen.nombre_genero asc

--Determinar el nombre y fecha de arriendo de la última película arrendada por película

SELECT pel.nombre_pelicula, MAX(pr.fecha_prestamo)
FROM PELICULA pel, COPIA_PELICULA cop, PRESTAMO pr
WHERE pr.id_pelicula = cop.id_pelicula 
	AND pr.numero_copia = cop.numero_copia
	AND cop.id_pelicula = pel.id_pelicula
GROUP BY pel.nombre_pelicula 

--determine el nombre y fecha de arriendo de la última película arrendada (Tarea)
