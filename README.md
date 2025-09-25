//# MR-Implementacion-BaseDatos
//Use the concepts of the relational model and the SQL DML (Data Management Language) and consult the information in the database engines (Discografia)




/*TALLER #2 - Carlos Beltrán*/
USE Discografia

 SELECT * FROM Pais;
 SELECT * FROM Tipo;
 SELECT * FROM Ritmo;
 SELECT * FROM Formato;
 SELECT * FROM Medio;
 SELECT * FROM Idioma;
 SELECT * FROM Compositor;
 SELECT * FROM Cancion;
 SELECT * FROM Interprete;
 SELECT * FROM Interpretacion;
 SELECT * FROM CancionCompositor;
  SELECT * FROM Album;


 /*Pregunta a:
 ¿Cuántas canciones ha compuesto "JUANES"?
 Tener en cuenta que el nombre del compositor viene completo
 (usar función CHARINDEX)*/

  SELECT 
    c.Nombre AS Compositor,
    COUNT(cc.IdCancion) AS TotalCanciones,
    STRING_AGG(ca.Titulo, ', ') AS Canciones
FROM Compositor c
INNER JOIN CancionCompositor cc ON c.Id = cc.IdCompositor
INNER JOIN Cancion ca ON cc.IdCancion = ca.Id
WHERE CHARINDEX('JUANES', c.Nombre) > 0
GROUP BY c.Nombre;


/*Pregunta b:
 ¿Qué interpretaciones se tienen de la canción "Lluvia" y en qué ritmos?*/
 
 SELECT 
    c.Titulo AS Cancion,
    i.IdInterprete AS Interpretacion,
    r.Ritmo
FROM Cancion c
INNER JOIN Interpretacion i ON c.Id = i.IdCancion
INNER JOIN Ritmo r ON i.IdRitmo = r.Id
WHERE c.Titulo = 'Lluvia';



SELECT 
    c.Titulo AS Cancion,
   inter.Nombre AS Interprete_Nombre,
    r.Ritmo
FROM Cancion c
INNER JOIN Interpretacion i ON c.Id = i.IdCancion
INNER JOIN Ritmo r ON i.IdRitmo = r.Id
INNER JOIN Interprete inter ON i.IdInterprete = inter.Id
WHERE c.Titulo = 'Lluvia';

  
 /*Pregunta c:
 ¿Qué canciones hay con el mismo intérprete y Compositor del ritmo "Balada"?
Tener en cuenta que solo aplica para solistas y que el nombre del compositor viene
completo (nombre de pila y seudónimo) mientras que el del interprete es el seudónimo solamente.*/

SELECT 
    ca.Titulo AS Cancion,
    i.Nombre AS Interprete,
    co.Nombre AS Compositor,
    r.Ritmo
FROM Cancion ca
INNER JOIN Interpretacion it ON ca.Id = it.IdCancion
INNER JOIN Interprete i ON it.IdInterprete = i.Id
INNER JOIN Tipo t ON i.IdTipo = t.Id
INNER JOIN Ritmo r ON it.IdRitmo = r.Id
INNER JOIN CancionCompositor cc ON ca.Id = cc.IdCancion
INNER JOIN Compositor co ON cc.IdCompositor = co.Id
WHERE r.Ritmo = 'Balada'
  AND t.Tipo = 'Solista'
  AND CHARINDEX(i.Nombre, co.Nombre) > 0;


/*Pregunta d:
 ¿Listar los países que tienen grupos del ritmo "Salsa"?*/

SELECT DISTINCT P.Pais
FROM Pais P
INNER JOIN Interprete I ON P.Id = I.IdPais
INNER JOIN Interpretacion IT ON I.Id = IT.IdInterprete
INNER JOIN Ritmo R ON IT.IdRitmo = R.Id
WHERE R.Ritmo = 'Salsa';


	  /* SELECT COUNT(*) AS Totalcanciones
	   FROM Interpretacion where IdRitmo = 3;*/


 /*Pregunta e:
 ¿Quiénes interpretanlas canciones "Candilejas" y "Malaguena"?*/

SELECT DISTINCT I.Nombre AS Interprete, C.Titulo AS Cancion
FROM Interprete I
INNER JOIN Interpretacion IT ON I.Id = IT.IdInterprete
INNER JOIN Cancion C ON IT.IdCancion = C.Id
WHERE C.Titulo IN ('Candilejas', 'Malaguena')
ORDER BY C.Titulo ASC;


 /*Pregunta f:
 ¿Listar artistas que son intérpretes y compositores a la vez y con cuantas canciones compuestas e interpretadas?*/

 SELECT 
    I.Nombre AS Artista,
    COUNT(DISTINCT CC.IdCancion) AS CancionesCompuestas,
    COUNT(DISTINCT IT.Id) AS CancionesInterpretadas
FROM Interprete I
INNER JOIN Compositor C 
       ON I.Nombre = C.Nombre  
LEFT JOIN CancionCompositor CC 
       ON C.Id = CC.IdCompositor
LEFT JOIN Interpretacion IT 
       ON I.Id = IT.IdInterprete
GROUP BY I.Nombre
HAVING COUNT(DISTINCT CC.IdCancion) > 0 
   AND COUNT(DISTINCT IT.Id) > 0
ORDER BY I.Nombre;




