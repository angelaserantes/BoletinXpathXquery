# Boletin Xpath Xquery

## Consultas con libro.xml

1. Títulos ordenados
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return $x/titulo 
```
Con la expresión for $x in se recorre cada elemento libro y con la cláusula return se devuelven los títulos.

2. Títulos ordenados por el nombre del primer autor
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
order by $x/autores/autor[1]/nombre
return $x/titulo
```

- "for $x in": establece una variable llamada "x" y recorre cada elemento "libro" en el archivo XML "libros.xml".
- "doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")": especifica la ruta del archivo XML.
- "/libros/libro": especifica la ruta de acceso para seleccionar los elementos "libro" en el archivo XML.
- "order by": ordena los elementos "libro" seleccionados por el nombre del primer autor de cada libro.
-  "$x/autores/autor[1]/nombre": especifica la ruta de acceso para seleccionar el nombre del primer autor de cada libro.
- "return": devuelve el valor de cada elemento "titulo" encontrado en la consulta.
- "$x/titulo": especifica la ruta de acceso para seleccionar los elementos "titulo" de cada libro.

3. Nombre y apellido del primer autor de cada libro
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return concat("Libro: ", $x/titulo, ", Autor: ", $x/autores/autor[1]/nombre, " ", $x/autores/autor[1]/apellido)
```

La expresión "for $x in" establece una variable llamada "x" y así se recorre cada elemento "libro"

Después, se utiliza la función "concat" para combinar cadenas de texto y crear una frase que incluye el título del libro y el nombre y apellido del primer autor. La función "concat" une las cadenas.

4. Título y número de autores de cada libro
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return concat("Libro: ", $x/titulo, ", Número de autores: ", count($x/autores/autor))
```
Se trabaja igual que en las anteriores consultas pero en este caso la función "concat" se emplea para concatenar el título del libro y el número de autores. La función "count" se emplea para contar el número de elementos "autor" que son hijos de "autores" de cada libro.


5. Libros que tienen varios autores y libros que tienen un autor
```
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
where count($x/autores/autor) > 1
return concat("Libros con varios autores: ", $x/titulo)

Se analizan los elementos "libro" y se devuelve solo los que tienen más de un autor.

for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
where count($x/autores/autor) = 1
return concat("Libro con un solo autor: ", $x/titulo)

```
 Se analizan los elementos "libro" y se devuelven solo los 
 que tienen un solo autor.

6.  Libros cuyo autor es “Axel”
```

for $libro in /libros/libro[autores/autor[nombre = 'Axel']]
return $libro/titulo
```
Con [autores/autor[nombre = 'Axel']] se devuelven los libros cuyo autor es Axel de la expresión xpath especificada.

7.  Mostrar título y precio en una lista HTML
```
<ul>
{
for $x in doc("C:\Users\Angela\BoletinXpathXquery\libros.xml")/libros/libro
return <li>{concat("Libro: ", $x/titulo, ", Precio: ", $x/precio)}</li>
}
</ul>
```
Integrando la consulta en una etiqueta ul conseguimos enmarcar el título y precio en una lista desordenada

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
En este caso se integra en una tabla como si trabajaramos con un archivo html.

## Consultas con alumnos.xml

1. Mostrar el nombre de los alumnos aprobados.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 5
return $alumno/nombre 
```
Esta consulta recorre cada elemento alumno y muestra aquellos que han aprobado (>=5)

2. Mostrar el DNI y la nota de los alumnos que han aprobado
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 5
return concat("DNI: ", $alumno/@dni, ", Nota: ", $alumno/nota)
```
En este caso se añade el elemento dni y utilizamos concat para devolver tanto el dni como la nota de los alumnos que han aprobado


3. Indicar el nombre de los alumnos cuyas notas están entre 6 y 8 (ambas inclusive).
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
where $alumno/nota >= 6 and $alumno/nota <= 8
return $alumno/nombre
```
Esta consulta se diferencia de la primera del archivo alumnos.xml al especificar las notas que se buscan de esta forma: where $alumno/nota >= 6 and $alumno/nota <= 8

4.  Mostrar listado de nombres ordenados por apellidos.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
order by $alumno/apells
return $alumno/nombre
```
En esta caso con la expresión order by se ordenan las tuplas tal y como se le indica, en este caso por apellidos.

5. Mostrar nombres ordenados por DNI.
```
for $alumno in doc("C:\Users\Angela\BoletinXpathXquery\alumnos.xml")/alumnos/alumno
order by $alumno/@dni
return concat($alumno/@dni, ' - ', $alumno/nombre)
```
Al igual que la anterior, se utiliza order by para ordenarlos por dni y con concat devolemos el dni y el nombre de alumno.

6. Mostrar el Nº de cada alumno y su nombre.
```
for $i in (1 to count(/alumnos/alumno))
for $alumno in /alumnos/alumno[$i]
return <alumno num="{$i}">{$alumno/nombre} {$alumno/apells}</alumno>
```
Se crean dos expresiones for, la primera para poder crear el numero de cada alumno, y la segunda.

La segunda expresión "for" establece la variable "alumno" que selecciona cada elemento "alumno" en la posición que corresponde a la secuencia de números enteros generada por la primera expresión "for".