# Boletin Xpath Xquery

## Consultas con libro.xml

1. Títulos ordenados
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return $x/titulo 
```

2. Títulos ordenados por el nombre del primer autor
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
order by $x/autores/autor[1]/nombre
return $x/titulo
```


3. Nombre y apellido del primer autor de cada libro
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return concat("Libro: ", $x/titulo, ", Autor: ", $x/autores/autor[1]/nombre, " ", $x/autores/autor[1]/apellido)
```
4. Título y número de autores de cada libro
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return concat("Libro: ", $x/titulo, ", Número de autores: ", count($x/autores/autor))
```
5. Libros que tienen varios autores y libros que tienen un autor
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
where count($x/autores/autor) > 1
return concat("Libros con varios autores: ", $x/titulo)

for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
where count($x/autores/autor) = 1
return concat("Libro con un solo autor: ", $x/titulo)

```
6.  Libros cuyo autor es “Axel”
```
for $libro in /libros/libro[autores/autor[nombre = 'Axel']]
return $libro/titulo
```

7.  Mostrar título y precio en una lista HTML
```
<ul>
{
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return <li>{concat("Libro: ", $x/titulo, ", Precio: ", $x/precio)}</li>
}
</ul>
```

8.  Mostrar: título, isbn y precio en una tabla HTML
```
<table>
<tr>
<th>Título</th>
<th>ISBN</th>
<th>Precio</th>
</tr>
{
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return <tr><td>{$x/titulo}</td><td>{$x/isbn}</td><td>{$x/precio}</td></tr>
}
</table>

```

## Consultas con alumnos.xml

1. Mostrar el nombre de los alumnos aprobados.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 5
return $alumno/nombre 
```

2. Mostrar el DNI y la nota de los alumnos que han aprobado
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 5
return concat("DNI: ", $alumno/@dni, ", Nota: ", $alumno/nota)
```

3. Indicar el nombre de los alumnos cuyas notas están entre 6 y 8 (ambas inclusive).
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 6 and $alumno/nota <= 8
return $alumno/nombre
```

4.  Mostrar listado de nombres ordenados por apellidos.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
order by $alumno/apells
return $alumno/nombre
```

5. Mostrar nombres ordenados por DNI.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
order by $alumno/@dni
return concat($alumno/@dni, ' - ', $alumno/nombre)
```

6. Mostrar el Nº de cada alumno y su nombre.
```
for $i in (1 to count(/alumnos/alumno))
for $alumno in /alumnos/alumno[$i]
return <alumno num="{$i}">{$alumno/nombre} {$alumno/apells}</alumno>
```
