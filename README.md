# Apuntes de Postgre / PostGIS

<img src="https://programapa.files.wordpress.com/2021/07/predicados-espaciales-postgis.png" width="300" height="200" text-align: center></div>

Por Rober J

<details>
  <summary><strong>Introducción a las bases de datos</strong></summary><br>
<p>Una <strong>base de datos</strong> es un conjunto de información estructurada para poder acceder a ella fácilmente. En informática, la base de datos se almacena digitalmente y por medio de programas se accede a la información y se opera con ella. Su aspecto es el de una (o muchas) tabla:</p>
<figure ><table><thead><tr><th>Nombre del elemento</th><th >Barrio</th><th >Distrito</th><th >Fecha de instalación</th></tr></thead><tbody><tr><td >Farola</td><td >Acacias</td><td >Arganzuela</td><td >1995</td></tr><tr><td >Farola</td><td >Atocha</td><td >Arganzuela</td><td >1995</td></tr><tr><td >Buzón</td><td >Jerónimos</td><td >Retiro</td><td >2001</td></tr></tbody></table></figure>
<p>Para que la base de datos sea <strong>geográfica</strong> debe contar con información acerca de su <strong>posición </strong>en el espacio, es decir, debe representar alguna realidad territorial. Una posibilidad sería contar con dos columnas más: una para el dato de la coordenada X y otra para su coordenada Y:</p>
<figure ><table><thead><tr><th >Nombre del elemento</th><th >Barrio</th><th >Distrito</th><th >Fecha de instalación</th><th >Coordenada X</th><th>Coordenada Y</th></tr></thead><tbody><tr><td >Farola</td><td >Acacias</td><td >Arganzuela</td><td >1995</td><td >-3.704281</td><td>40.403497</td></tr><tr><td >Farola</td><td >Atocha</td><td >Arganzuela</td><td >1995</td><td >-3.692546</td><td>40.406163</td></tr><tr><td >Buzón</td><td >Jerónimos</td><td >Retiro</td><td >2001</td><td >-3.684449</td><td>40.407504</td></tr></tbody></table></figure>
<p>De este modo, con el programa adecuado es posible visualizar la información sobre un marco espacial en vez de quedarnos solo con la tabla. Bastará con conocer el sistema de referencia de las coordenadas (WGS 84 EPSG: 4326 en este caso) para que cada una de las filas se convierta en un punto posicionado en sus coordenadas, es decir, pueda <strong>representar su geometría</strong>. </p>
<p>El tamaño de las bases de datos puede ser tan grande que para  manipular la información se recurre a filtros complejos que afinen nuestras búsquedas y operaciones. En este contexto es donde suelen usarse los lenguajes de programación, tanto en programas <strong>gestores de bases de datos</strong> (SGBD) como Acess o Postgre como en Sistemas de Información Geográfica como QGIS. Entre estos lenguajes se encuentra <strong>SQL</strong>, diseñado específicamente para ser el lenguaje común de las bases de datos. </p>
<p><strong>PostgreSQL</strong>, o solo Postgre, es un software de código abierto de gestión de bases de datos que utiliza el <strong>lenguaje SQL</strong> para operar con la información, y entre sus virtudes está la de gestionar la <strong>información espacial</strong> o geometrías de los elementos almacenados en ella. </p>
<p>Para comenzar a usarlo, debemos <a rel="noreferrer noopener" href="https://www.postgresql.org/download/" target="_blank">descargarlo de su página principal</a>, familiarizarnos un poco con <strong>PgAdmin</strong>, la herramienta de Postgre que proporciona interfaz gráfica a nuestras bases de datos y desde la que se realizan la mayor parte de las operaciones, y aprender a <strong>manejar los aspectos fundamentales de SQL</strong>, los cuales trato en este post.</p>
<div ><figure ><img src="https://programapa.files.wordpress.com/2021/04/image-7.png?w=1024" alt=""  srcset="https://programapa.files.wordpress.com/2021/04/image-7.png?w=1024 1024w, https://programapa.files.wordpress.com/2021/04/image-7.png?w=150 150w, https://programapa.files.wordpress.com/2021/04/image-7.png?w=300 300w, https://programapa.files.wordpress.com/2021/04/image-7.png?w=768 768w, https://programapa.files.wordpress.com/2021/04/image-7.png 1366w" sizes="(max-width: 1024px) 100vw, 1024px" /><figcaption><em>Aspecto de pgAdmin</em></figcaption></figure>
<p>Sin embargo, para aplicar estas herramientas a la información geográfica para crear <strong>geodatabases</strong> y combinarlas con Sistemas de Información Geográfica como QGIS es necesario hacer uso de una <strong>extensión </strong>de Postgre llamada <strong>PostGIS </strong>que añade las funcionalidades necesarias para trabajar con esta clase de información y de las cuales hablo en otras entradas del blog.</p>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/11/logo_square_postgis.png?w=300" alt="" width="193" height="193" srcset="https://programapa.files.wordpress.com/2020/11/logo_square_postgis.png?w=193 193w, https://programapa.files.wordpress.com/2020/11/logo_square_postgis.png?w=150 150w, https://programapa.files.wordpress.com/2020/11/logo_square_postgis.png 300w" sizes="(max-width: 193px) 100vw, 193px" /><figcaption><em>Logo oficial de PostGIS</em></figcaption></figure>
<p>Por<strong> último, la documentación oficial y otros enlaces de utilidad para Postgre, SQL y PostGIS los recopilo en el siguiente post junto a otros muchos recursos:</p>
<p><strong>⚠ Algunas cuestiones básicas como las claves primarias o la integridad de los datos aún no las he tratado en este post</strong>(proximamente)</p>
 
</details>
  
## DDL - Lenguaje de definición de datos 👷‍♀️
  
<details>
  <summary><strong>Tablas</strong></summary><br>
  
  <div ><figure ><img src="https://programapa.files.wordpress.com/2020/11/tablas.jpg?w=300" alt="" srcset="https://programapa.files.wordpress.com/2020/11/tablas.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/11/tablas.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/11/tablas.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p >Las tablas son la estructura básica de almacenamiento de PostgreSQL y en general de los datos en las <a href="https://medium.com/@eugeniomendoza/c%C3%B3mo-saber-si-necesitas-una-base-de-datos-nosql-b6cfd5bb7d9b" target="_blank" rel="noreferrer noopener">bases de datos relacionales</a>.</p>
<p>Constan de dos elementos básicos: filas y columnas</p>
<figure ><table><thead><tr><th >ID</th><th >Elemento </th><th >Barrio</th><th >Fecha</th></tr></thead><tbody><tr><td >0001</td><td >Farola</td><td >Acacias</td><td >1995</td></tr><tr><td >0002</td><td >Farola</td><td >Atocha</td><td >1995</td></tr><tr><td >0003</td><td >Buzón</td><td >Jerónimos</td><td >2001</td></tr></tbody></table></figure>
<ul><li>Las <strong>filas </strong>son cada uno de los registros de la tabla, en este caso elementos urbanos.</li><li>Las <strong>columnas </strong>son los distintos datos que se almacenan de cada elemento, en este caso el barrio y distrito al que pertenecen junto a su fecha de instalación.</li></ul>
<p>Además las tablas cuentan también con <a rel="noreferrer noopener" href="https://programapa.wordpress.com/2020/09/24/restricciones-en-sql/" target="_blank">restricciones</a>, tanto para toda la tabla como para algunas de sus columnas.</p>
<p>Forman parte del lenguaje de definición de datos (DDL) de Postgre, es decir, de las herramientas SQL para armar la estructura de la base de datos como son también los <a href="https://programapa.wordpress.com/2020/09/22/lista-de-instrucciones-basicas-de-sql/">esquemas</a> o los <a href="https://programapa.wordpress.com/2020/11/05/dominios/">dominios</a></p>


<h3><strong>Crear tablas</strong></h3>
<p>En una base de datos PostgreSQL pueden crearse tantas tablas como se necesite. La <strong>sintaxis</strong> básica para ello es:</p>
<pre>
CREATE TABLE &#91;IF NOT EXISTS] esquema.nombre_tabla(
nombre_columna1 tipo(n) &#91;RESTRICCIÓN],
nombre_columna2 tipo(n) &#91;RESTRICCIÓN],
nombre_columna3 tipo(n) &#91;RESTRICCIÓN],
&#91;RESTRICCIÓN DE TABLA],
&#91;RESTRICCIÓN DE TABLA]
);
</pre>
<ul><li><strong>Crea una tabla</strong> especificando un nombre junto al esquema al que van a pertenecer.</li><li>Con IF NOT EXISTS comprueba que el nombre no esté repetido para poder crearla.</li><li>Entre paréntesis se especifica el nombre de la columna<em>, </em>el <a href="https://programapa.wordpress.com/2020/10/01/tipos-de-datos-en-sql/">tipo de dato</a> y su longitud (n), y las <a href="https://programapa.wordpress.com/2020/09/24/restricciones-en-sql/">restricciones</a> que tendrá cada columna o toda la tabla.</li></ul>
<p>Por ejemplo, para crear la tabla del inicio, haríamos lo siguiente: </p>
<pre>
CREATE TABLE miesquema.Urbanismo (
ID char(4) PRIMARY KEY,
Elemento varchar(15),
Barrio varchar(15),
Fecha date
);
</pre>

<h3><strong>Modificar tablas y columnas</strong></h3>
<p>Podemos cambiar las propiedades de la tabla o de algunas de sus columnas combinando ALTER TABLE con alguna sentencias según lo que queramos modificar:</p>

<h4><strong>Cambiar nombre a la tabla</strong></h4>
<pre>
ALTER TABLE &#91;IF EXISTS] tabla
 RENAME TO nuevo_nombre
</pre>
<p>IF EXISTS es opcional y evita que de error en caso de no existir la tabla (lo cual detendría toda la ejecución del código)</p>

<h4><strong>Cambiar nombre de la columna</strong></h4>
<pre>
ALTER TABLE tabla RENAME COLUMN columna TO nuevo_nombre
</pre>

<h4><strong>Asignar un nuevo esquema a la tabla</strong></h4>
<pre>
ALTER TABLE tabla SET SCHEMA nombre_esquema
</pre>

<h4>Añadir una nueva columna</h4>
<pre>
ALTER TABLE tabla ADD COLUMN nombre_columna tipo(n) &#91;RESTRICCIÓN]
</pre>
<p>Se indica su nombre, el <a href="https://programapa.wordpress.com/2020/10/01/tipos-de-datos-en-sql/">tipo de dato</a> y opcionalmente sus <a href="https://programapa.wordpress.com/2020/09/24/restricciones-en-sql/">restricciones</a></p>

<h4><strong>Eliminar columnas</strong></h4>
<pre>
ALTER TABLE tabla DROP COLUMN columna &#91;RESTRICT/CASCADE]
</pre>
<p>Con RESTRICT (por defecto) da error en caso de haber datos en las columnas.</p>
<p>CASCADE la borra aunque contenga datos.</p>

<h4><strong>Añadir y quitar expresiones asociadas a las columnas</strong></h4>
<pre>
ALTER TABLE tabla ALTER COLUMN columna SET DEFAULT expresión
</pre>

<h4><strong>Cambiar el tipo de dato que puede almacenar la columna indicada</strong></h4>
<pre>
ALTER TABLE tabla ALTER COLUMN columna &#91;SET DATA] TYPE tipo(n)
</pre>

<h4><strong>Quitar expresión de la tabla</strong></h4>
<pre>
ALTER TABLE tabla ALTER COLUMN columna DROP DEFAULT
</pre>

<h4><strong>Aceptar o no datos nulos</strong></h4>
<pre>
ALTER TABLE tabla ALTER COLUMN columna { SET / DROP } NOT NULL
</pre>

<h4><strong>Añadir una restricción para toda la tabla</strong></h4>
<pre>
ADD CONSTRAINT nombre_restricción RESTRICCIÓN
</pre>

<h4><strong>Añadir una restricción en una columna</strong></h4>
<pre>
ADD CONSTRAINT nombre_restricción RESTRICCIÓN(columna)
</pre>

<h4><strong>Borrar restricción</strong></h4>
<pre>
DROP CONSTRAINT nombre_restricción &#91; RESTRICT / CASCADE ]
</pre>
<p>Con RESTRICT (por defecto) da error en caso de haber datos dependientes de la restricción</p>
<p>CASCADE borra la restricción junto a los datos dependientes de ella</p>


<h3><strong>Borrar las tablas o su contenido</strong></h3>
<p>Se puede tanto borrar completamente una tabla como simplemente dejarla limpia usando las siguientes cláusulas:</p>

<h4><strong>Borrar una o varias tablas (separadas por comas y en ese orden)</strong></h4>
<pre>
DROP TABLE &#91;IF EXISTS] tabla &#91;, tabla2]&#91;RESTRICT / CASCADE]
</pre>
<p>Con IF EXISTS se evita error en caso de no existir.</p>
<p>Por defecto se ejecuta RESTRICT para dar error en caso de existir objetos dependientes de las tablas.</p>
<p>Si se indica CASCADE borrará todas las tablas dependientes.</p>

<h4><strong>Borrar todos los registros de la tabla sin modificar su estructura</strong></h4>
<pre>
TRUNCATE TABLE tabla &#91;RESTRICT / CASCADE]
</pre>
<p>Con RESTRICT (por defecto) se genera error en caso de existir claves externas que la hagan referencia.</p>
<p>CASCADE vacía todas las tablas conectadas por la clave externa.</p>
  
  
</details>
<details>
  <summary><strong>Esquemas</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/09/esquemas.jpg?w=300" alt="" srcset="https://programapa.files.wordpress.com/2020/09/esquemas.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/09/esquemas.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/09/esquemas.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Los esquemas de PostgreSQL son conjuntos de datos o tablas propiedad de uno o varios usuarios de la base de datos.</p>
<p>Forman parte del lenguaje de definición de datos (DDL) de Postgre, es decir, de las herramientas SQL para crear y modificar los objetos que estructuran las bases de datos.</p>

<h3><strong>Crear un esquema</strong></h3>
<p>Basta con usar CREATE SCHEMA y asignarle un nombre:</p>
<pre>
CREATE SCHEMA nombre_esquema
</pre>

<h3><strong>Modificar un esquema</strong></h3>
<pre>
ALTER SCHEMA esquema RENAME TO nuevo_nombre
</pre>
<p>Cambiar el nombre del esquema</p>
<pre>
ALTER SCHEMA esquema OWNER TO nuevo_usuario
</pre>
<p>Cambiar el propietario del esquema</p>
<pre>
&#91;AUTHORIZATION usuario]
</pre>
<p>Especificar usuario propietario del nuevo esquema distinto al creador.</p>

<h3><strong>Borrar un esquema</strong></h3>
<pre>
DROP SCHEMA &#91;IF EXISTS] esquema &#91;RESTRICT / CASCADE]
</pre>
<p>Con IF EXISTS evitamos que se genere un error que detenga el proceso en caso de no existir el esquema.</p>
<p>Por defecto se ejecuta RESTRICT, generando un error en caso de existir información en el esquema.</p>
<p>Con CASCADE se elimina todo.</p>

  
</details>
<details>
  <summary><strong>Restricciones</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/09/constraints_sql-3.png?w=300" alt="" class="wp-image-3102" srcset="https://programapa.files.wordpress.com/2020/09/constraints_sql-3.png?w=300 300w, https://programapa.files.wordpress.com/2020/09/constraints_sql-3.png?w=150 150w, https://programapa.files.wordpress.com/2020/09/constraints_sql-3.png 383w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Las restricciones de PostgreSQL son reglas que deben cumplir los datos para poder incorporarlos a las columnas que se encuentren bajo dicha restricción.</p>
<p>Con ellas podemos evitar que se inserten valores nulos, geometrías que no cumplan cierto estándar de calidad o datos que se excedan de los límites que les marquemos.</p>
<p>Existen restricciones que afectan a las columnas y otras que se aplican a toda una tabla.</p>

<p><a id="restriccion_columna"></a></p>
<h3><strong>Restricciones de columna</strong></h3>
<p >Afectan solo a la columna indicada.</p>
<figure ><table><tbody><tr><td>NOT NULL</td><td>No se admiten valores nulos en la columna. <br><br>Obliga a rellenar la fila con valores permitidos si quiere agregarse a la tabla.</td></tr><tr><td>UNIQUE</td><td>El dato de una celda no puede repetirse en la columna. <br><br>Funciona a modo de clave alternativa.</td></tr><tr><td>PRIMARY KEY</td><td>Establece que la columna es la clave primaria de la tabla, por lo que no podrá contener valores nulos ni valores repetidos.</td></tr><tr><td>REFERENCES <em>tabla</em>(<em>columna</em>)</td><td>Solo pueden introducirse en esa columna datos que ya existan previamente en otra <em>tabla.</em> <br><br>Si no se especifica la <em>columna </em>por defecto buscará en la otra tabla una columna con el mismo nombre que a la que se le aplica la restricción. </td></tr><tr><td>CHECK (<em>condiciones</em>)</td><td>Los valores que se introduzcan en la columna deberán cumplir las <em>condiciones </em>que se especifiquen mediante fórmulas matemáticas o lógicas booleanas (TRUE o FALE)</td></tr></tbody></table></figure>

<h3><strong>Restricciones de tabla</strong></h3>
<p >Afectan a toda la tabla o a varias de sus columnas. </p>
<figure ><table><tbody><tr><td>UNIQUE (columna1,columna2&#8230;)</td><td>No se permite que se repitan combinaciones de valores en las filas: si la fila 1 tiene valores <strong>ab </strong>la fila 2 tendrá que ser <strong>aa, bb, ba, bc&#8230; </strong>pero no <strong>ab.</strong></td></tr><tr><td>FOREIGN KEY (columna1,columna2&#8230;) REFERENCES tabla(columna1,columna2&#8230;)</td><td>Establece a las columnas indicadas como claves foráneas para otra tabla. <br><br>Si las columnas de la otra tabla se llaman igual no es necesario especificar el nombre de las columnas a las que referencia.</td></tr><tr><td>CHECK (condiciones)</td><td>La tabla debe ajustarse a las condiciones que se especifiquen mediante fórmulas matemáticas o lógicas booleanas (TRUE o FALSE)</td></tr><tr><td>PRIMARY KEY (columna1,columna2&#8230;)</td><td>Cuando queramos establecer como clave primaria una combinación de columnas, deberá usarse como restricción de tabla.</td></tr></tbody></table></figure>

<h3><strong>Crear una restricción de tabla</strong></h3>
<p>Existen dos maneras de crear una restricción:</p>
<ul><li>Añadir restricciones de tabla y de columna <a href="https://programapa.wordpress.com/2020/11/04/tablas/">en el momento de crear una tabla</a></li><li>Añadirlas posteriormente utilizando ALTER TABLE:</li></ul>

<h4><strong>Añadir restricción a una tabla</strong></h4>
<pre>
ALTER TABLE &#91; IF EXISTS ] tabla ADD CONSTRAINT nombre_restricción RESTRICCIÓN
</pre>

<h4><strong>Añadir restricción a una columna</strong></h4>
<pre>
ALTER TABLE &#91; IF EXISTS ] tabla ADD CONSTRAINT nombre_restricción RESTRICCIÓN (columna)
</pre>
<p>En ambos casos se le asigna un nombre a la restricción para poder identificarla fácilmente. Las restricciones existentes para una tabla pueden verse desde el menú lateral de pgAdmin.</p>

<h3><strong>Borrar una restricción de tabla</strong></h3>
<p>Para borrar una una restricción hay que usar la sentencia modificadora de tablas ALTER TABLE e indicar el nombre que el usuario le dio a la restricción que se va a borrar:</p>
<pre>
ALTER TABLE &#91; IF EXISTS ] tabla DROP CONSTRAINT nombre_restricción &#91; RESTRICT / CASCADE ]
</pre>
<p>Con IF EXISTS no da error en caso de no existir la tabla</p>
<p>Nombre_restricción es el nombre que el usuario le dio a la restricción</p>
<p>Con RESTRICT (por defecto) da error en caso de haber datos dependientes de la restricción. CASCADE borra la restricción junto a los datos dependientes de ella</p>
  
  
</details>
<details>
  <summary><strong>Dominios</strong></summary><br>
  
  <div ><figure ><img src="https://programapa.files.wordpress.com/2020/11/dominios.jpg?w=300" alt="" class="wp-image-2926" srcset="https://programapa.files.wordpress.com/2020/11/dominios.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/11/dominios.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/11/dominios.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p >En PostgreSQL los dominios son <strong>conjuntos de <a href="https://programapa.wordpress.com/2020/09/24/restricciones-en-sql/">restricciones</a></strong> que se aplican a una serie de datos para adaptarlos a nuestras necesidades.</p>
<p>Forman parte del lenguaje de definición de datos (DDL) de Postgre, es decir, de las herramientas SQL para armar la estructura de la base de datos.</p>

<h3><strong>Crear dominios</strong></h3>
<pre>
CREATE DOMAIN nombre_dominio &#91;AS] tipo(n)
&#91;DEFAULT expresión]
&#91;CONSTRAINT nombre_restricción RESTRICCIÓN]
{ NOT NULL / NULL / CHECK (expresión) }
</pre>
<p><strong>Estructura para crear un nuevo dominio</strong> especificando un nombre de nuestra elección y el <a href="https://programapa.wordpress.com/2020/10/01/tipos-de-datos-en-sql/">tipo de datos</a> (float, text, date&#8230;) en el que se basará el dominio.</p>
<p>Con DEFULT establecemos la <em>expresión </em>que defina los valores por defecto de las columnas que operen bajo el nuevo dominio creado. Los valores deben coincidir con el tipo de datos especificado arriba. Si no se especifica, el valor por defecto será NULL.</p>
<p>Mediante CONSTRAINT asignamos <a href="https://programapa.wordpress.com/2020/09/24/restricciones-en-sql/">restricciones</a> y opcionalmente un nombre a la restricción de tabla.</p>
<p>Con NULL y NOT NULL se indica si se aceptan valores nulos o no (por defecto los acepta)</p>
<p>CHECK nos permite establecer una expresión booleana (verdadero o falso) para lanzar un error en caso de que no se cumplan los requisitos establecidos al actualizar un campo de acuerdo a la restricción.</p>

<h3><strong>Modificar Dominos</strong></h3>
<p>Para modificar un dominio debe combinarse ALTER DOMAIN con alguna cláusulas en función de lo que queramos modificar:</p>

<h4><strong>Indicar al dominio un nuevo valor por defecto o eliminarlo</strong></h4>
<pre>
ALTER DOMAIN dominio
 { SET DEFAULT expresión / DROP DEFAULT }
</pre>
<p>No afecta a los registros ya creados.</p>
<p>DROP DEFAULT borra el valor por defecto.</p>

<h4><strong>Borra una restricción de dominio </strong></h4>
<pre>
ALTER DOMAIN dominio
 DROP CONSTRAINT &#91; IF EXISTS ] restricción &#91; RESTRICT / CASCADE]
</pre>
<p>Da opción a que salga error si hay objetos dependientes con RESTRICT o que por el contrario los borre con CASCADE.</p>

<h4><strong>Cambiar el nombre de una restricción</strong></h4>
<pre>
ALTER DOMAIN dominio
 RENAME CONSTRAINT restricción TO nuevo_nombre
</pre>

<h4><strong>Cambia al propietario del dominio a uno nuevo, al actual o al que inicie sesión</strong></h4>
<pre>
ALTER DOMAIN dominio
 OWNER TO { nuevo_usuario / CURRENT_USER / SESSION_USER }
</pre>

<h4><strong>Cambia el que se acepten o no valores nulos</strong></h4>
<pre>
ALTER DOMAIN dominio
 { SET / DROP } NOT NULL
</pre>

<h4><strong>Añade una nueva restricción al dominio</strong> siguiendo la estructura de CREATE DOMAIN</h4>
<pre>
ALTER DOMAIN dominio
 ADD restricción
</pre>

<h4><strong>Cambia el nombre del dominio</strong></h4>
<pre>
ALTER DOMAIN dominio
 RENAME TO nuevo_nombre
</pre>

<h4><strong>Cambia el esquema al que pertenece el dominio</strong></h4>
<pre>
ALTER DOMAIN dominio
 SET SCHEMA nuevo_esquema
</pre>

<h3><strong>Borrar dominios</strong></h3>
<pre><code>DROP DOMAIN &#091; IF EXISTS ] dominio &#091;,dominio2] &#091; CASCADE / RESTRICT ]</code></pre>
<p>Se pueden borrar varios dominios separándolos por comas y en ese orden</p>
<p>Con IF EXISTS (solo en PostgreSQL) se evita error en caso de no existir.</p>
<p>Por defecto se ejecuta RESTRICT para dar error en caso de existir objetos dependientes del dominio. CASCADE borra todos los objetos dependientes del dominio.</p>
  
</details>
<details>
  <summary><strong>Tipos de datos</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/elefentesql-copia-2.jpg?w=300" alt="" class="wp-image-3113" srcset="https://programapa.files.wordpress.com/2020/10/elefentesql-copia-2.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/elefentesql-copia-2.jpg?w=150 150w, https://programapa.files.wordpress.com/2020/10/elefentesql-copia-2.jpg 535w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Cada columna en una base de datos de <strong>PostgreSQL </strong>puede almacenar un tipo de dato concreto que debe ser especificado en función de la clase de información que queramos introducir. </p>
<p>La elección del tipo de dato debe hacerse en el proceso inicial del diseño de la base de datos. Ahorrará espacio en disco, agilizará las consultas y operaciones y evitará engorrosas modificaciones a posteriori.</p>

<h3><strong>Caracteres y texto</strong></h3>
<figure ><table><thead><tr><th>Tipo</th><th>Tamaño</th><th>Descripción</th></tr></thead><tbody><tr><td>char(n)</td><td>n bytes</td><td>Reserva espacio en disco para almacenar &#8216;n&#8217; caracteres (entre 1 y 8000) según la codificación usada.</td></tr><tr><td>varchar(n)</td><td>n bytes</td><td>Almacena hasta n caracteres según la codificación sin reservar espacio para ello.</td></tr><tr><td>text</td><td>1 &#8211; 2.147.483.647 bytes</td><td>Puede almacenar texto hasta el límite en bytes permitido.</td></tr></tbody></table><figcaption><em>❗ este tipo de datos debe introducirse &#8216;entrecomillado&#8217;</em></figcaption></figure>

<h3><strong>Números enteros</strong></h3>
<figure ><table><tbody><tr><td>smallint</td><td>2 bytes</td><td>Valores entre -32.768 y 32767</td></tr><tr><td>int</td><td>4 bytes</td><td>Valores entre -2.147.483.648 y 2.147.483.647</td></tr><tr><td>bigint</td><td>8 bytes</td><td>Valores entre 2<sup>-63</sup> y  2<sup>63</sup>&nbsp;</td></tr><tr><td>serial</td><td>4 bytes</td><td>Valores autoincrementables entre -2.147.483.648 y 2.147.483.647</td></tr></tbody></table></figure>

<h3><strong>Números decimales</strong></h3>
<figure ><table><tbody><tr><td>numeric(p,s)</td><td>Variable</td><td>Almacena &#8216;p&#8217; dígitos (precisión) de los cuales &#8216;s&#8217; son decimales (escala)<br>Son valores exactos, ideales para el cálculo.</td></tr><tr><td>real</td><td>4 bytes</td><td>Almacena números reales con hasta 6 posiciones decimales.<br>Valores inexactos.</td></tr><tr><td>float8<br>double precision</td><td>8 bytes</td><td>Almacena números reales con hasta 15 posiciones decimales.<br>Valores inexactos.</td></tr></tbody></table></figure>

<h3><strong>Fechas</strong></h3>
<figure ><table><tbody><tr><td>date</td><td>4 bytes</td><td>Fechas en  calendario gregoriano</td></tr><tr><td>time</td><td>8 bytes</td><td>Almacena la hora con resolución de microsegundo</td></tr><tr><td>timestamp</td><td>8 bytes</td><td>Fecha y hora entre el 4713 a.C. y 294276 d.C. </td></tr><tr><td>timestampz</td><td>8 bytes</td><td>Fecha y hora incluyendo zona horaria entre el 4713 a.C. y 294276 d.C.</td></tr></tbody></table><figcaption><em>❗ este tipo de datos debe introducirse &#8216;entrecomillado&#8217;</em></figcaption></figure>

<h3><strong>Valores lógicos (booleanos)</strong></h3>
<figure ><table><tbody><tr><td>boolean</td><td>1 bit</td><td>Valores que se convierten a TRUE (1, yes, t, y, true) o a FALSE (0, no, f, n, false)</td></tr></tbody></table></figure>

<h3><strong>Geometrías (solo con PostGIS)</strong></h3>
<figure ><table><tbody><tr><td>geometry(tipo, SRC)</td><td>Variable</td><td>Almacena objetos geométricos simples (<em>Simple Feature</em>)<br><br>El <a href="https://programapa.wordpress.com/2020/11/06/tipos-de-datos-espaciales/">tipo</a> puede ser POINT, MULTIPOINT, LINESTRING, MULTILINESTRING, POLYGON y MULTIPOLYGON<br><br>SRC es el código del <a rel="noreferrer noopener" href="https://spatialreference.org/" target="_blank">sistema de referencia</a></td></tr><tr><td>topogeometry(tipo, SRC, tolerancia)</td><td>Variable</td><td>Almacena objetos topogeométricos<br><br>Al igual que las geometrías, su <a href="https://programapa.wordpress.com/2020/11/06/tipos-de-datos-espaciales/">tipo</a> puede ser también POINT, MULTIPOINT, LINESTRING, MULTILINESTRING, POLYGON y MULTIPOLYGON y también requieren de un <a rel="noreferrer noopener" href="https://spatialreference.org/" target="_blank">sistema de referencia</a><br><br>Sin embargo, la topogeometría requiere de una tolerancia clúster para realizar los cálculos. Su valor se dará en unidades del sistema</td></tr></tbody></table></figure>
  
</details>
<details>
  <summary><strong>Operadores</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/elefentesql-copia.jpg?w=300" alt="" class="wp-image-3110" srcset="https://programapa.files.wordpress.com/2020/10/elefentesql-copia.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/elefentesql-copia.jpg?w=150 150w, https://programapa.files.wordpress.com/2020/10/elefentesql-copia.jpg 535w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Los operadores son usados en las consultas de <strong>PostgreSQL </strong>para manipular los datos de la base de datos. Permiten filtrar la información para obtener solo los registros que necesitamos y hacer operaciones con ellos.</p>

<h3><strong>Operadores aritméticos</strong></h3>
<p>Llevan a cabo operaciones matemáticas. El resultado es siempre un número.</p>
<figure ><table><thead><tr><th>Operador</th><th>Descripción</th><th>Ejemplo</th><th>Resultado</th></tr></thead><tbody><tr><td>+</td><td>Suma</td><td>5 + 3</td><td>8</td></tr><tr><td>&#8211;</td><td>Resta</td><td>2 &#8211; 2</td><td>0</td></tr><tr><td>*</td><td>Multiplicación</td><td>7 * 3</td><td>21</td></tr><tr><td>/</td><td>División</td><td>9 / 3</td><td>3</td></tr><tr><td>^</td><td>Elevación</td><td>6 ^ 2</td><td>36 (6*6)</td></tr><tr><td>%</td><td>Módulo: resto de una división entera</td><td>20 % 3<br></td><td>2 (20/3=18)</td></tr><tr><td>|/</td><td>Raíz cuadrada</td><td>|/ 81</td><td>9 (9*9=81)</td></tr><tr><td>||/</td><td>Raíz cúbica</td><td>||/125</td><td>5 (5*5*5=125)</td></tr><tr><td>!</td><td>Factorial</td><td>4!</td><td>24 (1*2*3*4)</td></tr><tr><td>!!</td><td>Factorial (prefijo)</td><td>!!4</td><td>24 (1*2*3*4)</td></tr><tr><td>@</td><td>Valor absoluto</td><td>@ -25<br>@25</td><td>25</td></tr></tbody></table></figure>
<p>Cuando varios operadores se encuentran dentro de una misma expresión, se ejecutan siguiendo un orden, denominado <strong>precedencia de operadores</strong>:</p>
<ol><li>Lo que se encuentre entre paréntesis () </li><li>Multiplicaciones, divisiones y módulos</li><li>Sumas y restas</li><li>Izquierda a derecha cuando exista igualdad de importancia</li></ol>

<h3><strong>Operadores de comparación</strong></h3>
<p>Comprueban si dos valores son iguales o arrojan el mismo resultado. Los resultados de los operadores de comparación son valores true o false dependiendo de si se cumple la comparación o no.</p>
<figure ><table><thead><tr><th>Operador</th><th>Descripción</th><th>Ejemplo </th><th><strong>Resultado</strong></th></tr></thead><tbody><tr><td>=</td><td>Igual que </td><td>1 + 2 = 3</td><td><span class="uppercase">true</span></td></tr><tr><td>!=</td><td>Distinto que</td><td>25 != 5 * 5</td><td>FALSE</td></tr><tr><td>&lt;&gt;</td><td>Distinto que (estándar)</td><td>25 &lt;&gt; 5 * 5</td><td>FALSE</td></tr><tr><td>&lt;</td><td>Menor que</td><td>7  &lt; 10</td><td>TRUE</td></tr><tr><td>&gt;</td><td>Mayor que</td><td>7 &gt; 10</td><td>FALSE</td></tr><tr><td>&lt;=</td><td>Menor o igual que</td><td>7 &lt;= 5</td><td>FALSE</td></tr><tr><td>&gt;=</td><td>Mayor o igual que</td><td>7 &lt;= 7</td><td>TRUE</td></tr><tr><td>!&lt;</td><td>No es menor que</td><td>9 !&lt; 8</td><td>TRUE</td></tr><tr><td>!&gt;</td><td>No es mayor que</td><td>9 !&gt; 7</td><td>FALSE</td></tr></tbody></table></figure>

<h3><strong>Operadores lógicos</strong></h3>
<p>Comprueban si una o varias expresiones son verdad o no. Su resultado son valores true o false. </p>
<p>Daremos los ejemplos con una tabla hipotética con los municipios de España, su población, la provincia y la comunidad autónoma a la que pertenecen.</p>
<figure ><table><thead><tr><th>Operador</th><th>Descripción</th><th>Ejemplo </th><th>Resultado</th></tr></thead><tbody><tr><td>ALL</td><td>TRUE cuando todas las expresiones son TRUE</td><td>(provincia = Zaragoza) ALL (población &lt; 2M)</td><td>TRUE (todos los municipios de Zaragoza tienen menos de 2M de habitantes)</td></tr><tr><td>AND</td><td>TRUE si dos expresiones son TRUE</td><td>(provincia = Zamora) AND (población &gt; 1M)</td><td>FALSE (Zamora existe pero no tiene ningún municipio con 1M de habitantes)</td></tr><tr><td>ANY/SOME</td><td>TRUE para los valores que coinciden con la consulta</td><td>10000 &lt; ANY (población)</td><td>TRUE para todos los municipios con más de 10000 habitantes</td></tr><tr><td>BETWEEN</td><td>TRUE para los valores dentro de un rango</td><td>población BETWEEN 1000 AND 10000</td><td>TRUE para los municipios que tengan entre 1000 y 10000 habitantes</td></tr><tr><td>EXISTS</td><td>TRUE cuando existe el valor en la base de datos</td><td>EXISTS Teruel</td><td>TRUE (Teruel existe)</td></tr><tr><td>IN</td><td>TRUE cuando los datos de un campo se encuentran en la lista</td><td>Provincia IN (&#8216;Jaén&#8217;,&#8217;Granada&#8217;)</td><td>TRUE para todos los municipios de Jaen y Granada.</td></tr><tr><td>NOT </td><td>TRUE cuando no se cumpla la condición. Invierte operadores como LIKE, IN, BETWEEN, EXISTS, IS&#8230;</td><td>NOT (población &gt; 10000)</td><td>TRUE para todos aquellos municipios con menos de 10000 habitantes.</td></tr><tr><td>OR </td><td>TRUE cuando al menos una expresión sea verdadera</td><td>(población &gt;= 5M) OR (provincia = Madrid)</td><td>TRUE. Pese a que ningún municipio de España tiene más de 5M de habitantes, existe la provincia de Madrid.</td></tr><tr><td>IS NULL<br></td><td>TRUE cuando exista algún en el registro. Puede combinarse con NOT.</td><td>Provincia IS NOT NULL</td><td>TRUE porque todos los municipios tendrán asignada una provincia.</td></tr></tbody></table></figure>

<h3><strong>Operadores de cadenas de caracteres</strong></h3>
<p>Permiten operar con texto o cadenas de caracteres, llamadas <em>string</em> en jerga informática.</p>
<figure ><table><thead><tr><th>Operador</th><th>Descripción</th><th>Ejemplo</th><th>Resultado</th></tr></thead><tbody><tr><td>+</td><td>Concatena texto</td><td>&#8216;Corre&#8217; + &#8216;caminos&#8217;</td><td>Correcaminos</td></tr><tr><td>||</td><td>Concatena texto con otros datos que no sean text</td><td>&#8216;Valor: &#8216; || 200</td><td>Valor: 200</td></tr><tr><td>substr</td><td>Extrae subcadenas de texto indicando posiciones.</td><td>substr(&#8216;Correcaminos&#8217;, 0,1,2,3,4)</td><td>Corre</td></tr><tr><td>LIKE</td><td>Compara fragmentos de texto y devuelve TRUE en caso de coincidencia</td><td>municipio LIKE &#8216;Alm&#8217;</td><td>True para municipios que contengan &#8216;alm&#8217; en su nombre: Almería, Almunia&#8230;</td></tr></tbody></table></figure>
  
</details>
<details>
  <summary><strong>Índices</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/indices-postgresql-1.png?w=266" alt="" class="wp-image-2137" srcset="https://programapa.files.wordpress.com/2020/10/indices-postgresql-1.png 266w, https://programapa.files.wordpress.com/2020/10/indices-postgresql-1.png?w=150 150w" sizes="(max-width: 266px) 100vw, 266px" /></figure>
<p>En PostgreSQL los índices son una selección de columnas de una tabla que hacen referencia a todos sus registros. Lo que se consigue con ello es reducir los tiempos de consulta, puesto que en vez de leer toda la tabla se leerá solo el índice, y éste referenciará el resto de campos de la tabla.</p>
<p>Si por ejemplo tenemos una base de datos geográficos para el sistema inteligente de una gran ciudad (Smart City) y queremos encontrar los datos de un solo tipo de bombillas que supondrían solo un 5% del total de miles que puede haber repartidas por toda la ciudad, con un índice en base a los tipos de bombilla nuestra consulta no tendría que recorrer toda la tabla, sino que se centraría en la columna de los tipos de bombilla y a partir de ahí sacaría todos los datos de las que coincidiesen.</p>
<p>Hay que tener en cuenta que los índices <strong>ocupan espacio en disco</strong>, así que no hay que volverse loco creyendo que con muchos índices nos irá todo más rápido porque puede ir en nuestra contra, además de que deben ser actualizados.</p>

<h3><strong>Crear un índice</strong></h3>
<p>PostgreSQL crea automáticamente un índice sobre el campo asignado como Primary Key. Este índice se llamará indice primario, pero pueden crearse más índices dependiendo del campo o expresión a la que queramos tener un acceso más rápido.</p>
<p>La sintaxis para crear un índice en PostgreSQL es la siguiente:</p>
<pre>
CREATE INDEX &#91;IF NOT EXISTS] nombre_del_índice 
ON tabla USING método
(columnas o expresión) 
TABLESPACE pg_default;
</pre>
<p><strong>CREATE INDEX </strong>es la sentencia básica para crear el índice y asignarle un nombre</p>
<p><strong>IF NOT EXISTS</strong> impide que salte error en caso de existir un índice con el mismo nombre que le estamos dando</p>
<p><strong>ON </strong>especifica la tabla de la que se va a hacer el índice</p>
<p><strong>USING</strong> asigna un <strong>método </strong>de indexación a la columna seleccionada o al resultado de una expresión que definamos</p>
<p><strong>TABLESPACE</strong> identifica el lugar donde se almacenará el índice. pg_default es el predeterminado de Postgre.</p>

<h3><strong>Métodos de indexación</strong></h3>
<p>Es el algoritmo que usa PostgreSQL para crear el índice. Si no se especifica ninguno será <strong>btree </strong>por defecto, que es el más común y que se adapta a la mayoría de situaciones.</p>
<p>Para bases de datos espaciales el que más nos interesa es <strong>gist</strong>. Recomiendo echar un vistazo al <a rel="noreferrer noopener" href="https://www.postgresql.org/docs/current/indexes-types.html" target="_blank">artículo sobre indexación espacial</a> de GeoTalleres y a <a rel="noreferrer noopener" href="http://www.sigdeletras.com/2019/creacion-de-indices-con-postgresql-postgis/" target="_blank">este otro artículo</a> sobre el uso de este tipo de índice, pero en resumidas cuentas para indexar geometrías gist es más rápido que b-tree y evita que en caso de existir geometrías muy complejas se supere el tamaño máximo que permite Postgre para un índice.</p>
<p>En la documentación oficial se detallan los<a rel="noreferrer noopener" href="https://www.postgresql.org/docs/current/indexes-types.html" target="_blank"> distintos tipos o métodos de indexación.</a> </p>

<h3><strong>Usar un índice</strong></h3>
<p>Una vez creado el índice, cuando hagamos consultas sobre esa tabla el propio Postgre lo usará para recorrerla.</p>
<p>Puede comprobarse la diferencia en tiempo de procesado de usar o no un índice mediante <strong>EXPLAIN ANALYSE</strong>, una función que devuelve el plan de ejecución de una consulta sin realizarla. Bastaría con usarlo en una consulta antes de crear el índice y volverlo a usar una vez creado.</p>

<h3><strong>Borrar un índice</strong></h3>
<p>Para borrar un índice basta con usar <strong>DROP INDEX:</strong></p>
<pre>
DROP INDEX nombre_del_índice
</pre>
  
</details>
<details>
  <summary><strong>Secuencias y serials</strong></summary><br>
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/serials_sql.jpg?w=300" alt="" class="wp-image-2275" srcset="https://programapa.files.wordpress.com/2020/10/serials_sql.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/serials_sql.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/10/serials_sql.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Las secuencias en <strong>PostgreSQL</strong> son una herramienta para generar números enteros sin que se repita ninguno. Estos números se asignan a campos numéricos que habitualmente son clave primaria o serials.</p>
<p>Si por ejemplo inventariamos los cientos de ejemplares de árbol que puede haber en un monte no vamos a darle un nombre único a cada uno, echaríamos mano de una secuencia para que a cada árbol que metamos en la base de datos se le asigne un ID único.</p>

<h3><strong>Crear secuencias</strong></h3>
<p>La sintaxis para crear secuencias es la siguiente:</p>
<pre>
CREATE SEQUENCE nombre_secuencia
&#91;START WITH x]
&#91;INCREMENT BY n]
&#91;MAXVALUE y]
&#91;MINVALUE z]
&#91;CYCLE];
</pre>
<ul><li><strong>CREATE SEQUENCE</strong> es la sentencia para crear la secuencia y asignarle el nombre que queramos</li><li><strong>START WITH</strong> asigna un valor X de tipo entero desde el que empezar</li><li><strong>INCREMENT BY</strong> asigna un valor N de tipo entero que se sumará al anterior</li><li><strong>MAXVALUE</strong> y <strong>MINVALUE</strong> establece los valores enteros máximo y mínimo que tendrá la secuencia</li><li><strong>CYCLE</strong> señala que la secuencia se repetirá una vez llegue al máximo</li></ul>

<p>Los parámetros anteriores pueden omitirse para tomar los siguientes <strong>valores por defecto</strong>:</p>
<ul><li>START WITH 1</li><li>INCREMENT BY 1</li><li>MIN VALUE -9223372036854775808</li><li>MAX VALUE 9223372036854775807</li><li>No se repetirá una vez alcance el máximo</li></ul>
<p>Se puede <strong>conocer cuál será el siguiente valor</strong> de la secuencia con NEXTVAL y usarlo para <strong>insertarlo</strong> en un nuevo registro.</p>
<p>Averiguar el siguiente valor de una secuencia:</p>
<pre>
SELECT NEXTVAL('nombre_secuencia');
</pre>
<p>Insertar un nuevo registro en el que el valor de la primera columna será el del siguiente de la serie:</p>
<pre>
INSERT INTO tabla
VALUES (nextval('nombre_serie'), valor_columna2...);
</pre>
<p>En este ejemplo creamos una tabla con ríos en la que a cada uno se le asigna un ID en base a una serie:</p>
<pre>
CREATE TABLE rios(
ID integer DEFAULT nextval('serie'),
nombre varchar(30),
longitud numeric
)
</pre>

<h3><strong>Modificar una secuencia</strong></h3>
<p>Al modificar una sentencia solo se alteran los siguientes valores que surjan de ella, no los ya creados. La sintaxis es:</p>
<pre>
ALTER SEQUENCE nombre_secuencia
&#91;START WITH x]
&#91;INCREMENT BY n]
&#91;MAXVALUE y]
&#91;MINVALUE z]
&#91;CYCLE];
</pre>

<h3><strong>Borrar una secuencia</strong></h3>
<p>Para borrar una secuencia la sintaxis es la siguiente:</p>
<pre>
DROP SEQUENCE nombre_secuencia &#91;CASCADE];
</pre>
  
</details>
<details>
  <summary><strong>Vistas de datos</strong></summary><br>
  
  <div><figure><img src="https://programapa.files.wordpress.com/2020/10/elefante-tv.png?w=150" srcset="https://programapa.files.wordpress.com/2020/10/elefante-tv.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/elefante-tv.png 271w" sizes="(max-width: 150px) 100vw, 150px" /></figure>
<p>Las vistas son una herramienta de <strong>PostgreSQL</strong> para almacenar consultas de información y visualizarlas modo de tabla virtual sin alterar en ningún momento la información de la base de datos.</p>
<p>Son muy útiles porque permiten visualizar conjuntos de datos de forma rápida sin alterar la información, permitiendo ocultarla o mostrarla según nuestra consulta. </p>
<p>También sirven para almacenar esas consultas, especialmente si son complejas, y acceder a ellas en todo momento, mejorando nuestra productividad.</p>
<p>Por último, pueden usarse para dar permisos a otros usuarios de solo visualización, evitando así que puedan modificar los datos.</p>
<p>A través de las funciones <a href="https://programapa.wordpress.com/2020/10/15/consultar-y-agregar-atributos-en-postgresql/">SELECT</a> y <a href="https://programapa.wordpress.com/2020/10/19/combinar-registros-de-tablas-diferentes-en-postgresql-con-join/">JOIN</a> entre otras podrán mostrar tanto conjuntos de filas y columnas de una taba como de varias tablas y resúmenes estadísticos. Así mismo también pueden combinarse las vistas entre sí y con otras tablas.</p>

<h3><strong>Crear vistas</strong></h3>
<p>Tan solo hay que seleccionar un conjunto de datos con <strong>CREATE VIEW</strong>:</p>
<pre>
CREATE VIEW nombre_vista as
SELECT &#91;sentencias select];
</pre>
<p>Se recomienda que los nombres de las vistas sean fácilmente identificables, añadiendo prefijos o sufijos como »vista» a la descripción de la misma.</p>
<p>Para <strong>crear una vista a partir de otra</strong> <strong>vista</strong>, bastaría con referirse a ella al momento de crearla:</p>
<pre>
CREATE VIEW nombre_vista2 as 
SELECT columna1, columna2 FROM nombre_vista1;
</pre>

<h3><strong>Visualizar datos de las vistas</strong></h3>
<p>Visualizarlo siempre que queramos con <a href="https://programapa.wordpress.com/2020/10/15/consultar-y-agregar-atributos-en-postgresql/">SELECT</a>, ya sea toda la vista o solo ciertas columnas o registros:</p>
<pre>
-- Selecciona toda la vista
SELECT * FROM nombre_vista;
-- Selecciona las columnas 1 y 2 de la vista
SELECT columna1, columna2 FROM nombre_vista;
-- Devuelve el valor máximo de la columna 3 de la vista
SELECT MAX(columna3) FROM nombre_vista;
</pre>

<h3><strong>Borrar una vista</strong></h3>
<p>Las vistas se borran con <strong>DROP VIEW</strong>:</p>
<pre>
DROP VIEW nombre_vista
</pre>
  
</details>
<details>
  <summary><strong>Funciones</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/funciones_sql-1.jpg?w=300" alt="" srcset="https://programapa.files.wordpress.com/2020/10/funciones_sql-1.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/funciones_sql-1.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/10/funciones_sql-1.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Las funciones en <strong>PostgreSQL </strong>son sentencias predefinidas por el usuario para no tener que repetir el mismo código una y otra vez. Con una vez que sean definidas tan solo habrá que introducir en el futuro las nuevas variables que serán aplicadas a dicha función.</p>
<p>Si por ejemplo solemos consultar en nuestra geodatabase los usos de suelo de España por provincias, no será necesario andar reescribiendo o copiando y pegando la misma consulta  una y otra vez, sino que podemos definir una función en la que tan solo introduciendo el nombre de la provincia que necesitemos.</p>
<p>PostgreSQL cuenta con <strong>funciones propias</strong> que <a rel="noreferrer noopener" href="https://www.postgresql.org/docs/13/functions.html" target="_blank">se detallan en la documentación oficial</a>, entre ellas las funciones de agregado que vemos en la entrada <a href="https://programapa.wordpress.com/2020/10/15/consultar-y-agregar-atributos-en-postgresql/">consultar y agregar atributos</a>. Conocerlas evita tener que crearlas, pero en muchas ocasiones tendremos que hacerlo según nuestras necesidades.</p>

<h3><strong>Crear una función</strong></h3>
<p>La sintaxis para crear una función es la siguiente:</p>
<pre>
CREATE &#91;OR REPLACE] FUNCTION nombre_función(parametros) 
RETURNS tipo_dato
AS 'expresión' 
language sql;
</pre>
<ul><li><strong>CREATE FUNCTION</strong> es la sentencia para crear funciones</li><li><strong>OR REPLACE</strong> es opcional y permite reescribir una función ya existente que tenga el mismo nombre</li><li>Los <strong>parámetros</strong> son los nombres que se le darán a las variables de la función junto a su tipo de dato. Conviene no repetir con nombres de columna.</li><li><strong>RETURNS</strong> especifica el tipo de dato que devolverá la función.</li><li><strong>AS </strong>define la expresión o función que tomará las variables definidas en los parámetros</li><li><strong>language sql</strong> indica que la función se ha escrito en SQL. Además de SQL pueden escribirse funciones en otros lenguajes como PL/PgSQL</li></ul>

<h4><strong>Ejemplo</strong></h4>
<p>Podemos poner como ejemplo la función que seleccionaría los usos del suelo en función de la provincia. Para crearla usaríamos el siguiente código:</p>
<pre>
CREATE OR REPLACE FUNCTION usos_provincia(varchar) RETURNS varchar
AS 'SELECT usos FROM tabla_usos WHERE provincia = $1'
language sql;
</pre>
<p>&#8216;varchar&#8217; es el tipo de argumento cuyo dato es introducido por el usuario y sustituirá a $1 en el SELECT.</p>
<p>En este caso indica que el usuario deberá introducir una cadena de texto de tipo variable.</p>
<p>$1 indica que se está tomando el primer argumento de la lista, en este caso &#8216;nombre&#8217;. Es muy útil si la función requiere de varios argumentos.</p>
<p>Para <strong>usar </strong>esta función y obtener los usos en Cáceres lo haríamos de la siguiente manera:</p>
<pre>
SELECT usos_provincia('Cáceres');
</pre>

<h3><strong>Eliminar funciones</strong></h3>
<p>La sintaxis para borrarlas es:</p>
<pre>
DROP FUNCTION &#91; IF EXISTS ] nombre_función&#91;(argumentos)]&#91;CASCADE]
</pre>
<p>Los <strong>argumentos </strong>solo habría que definirlos en caso de haber creado dos funciones distintas con el mismo nombre.</p>
<p><strong>IF EXISTS</strong> evita que se genere error en caso de no existir la función</p>
<p><strong>CASCADE </strong>borrará todos los elementos que dependan de la función que se va a eliminar.</p>
  
</details>
<details>
  <summary><strong>Triggers</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/trigger-sql.jpg?w=300" srcset="https://programapa.files.wordpress.com/2020/10/trigger-sql.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/trigger-sql.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/10/trigger-sql.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>En <strong>PostgreSQL</strong>, los triggers (desencadenantes o disparadores) son códigos que se ejecutarán de forma automática cuando se produzca un evento determinado.</p>
<p>Podemos programarlos para que se ejecuten:</p>
<ul><li><strong>antes o después</strong> de una sentencia <a href="https://programapa.wordpress.com/2020/10/05/lista-de-instrucciones-basicas-en-postgresql-dml/">SELECT, INSERT, DELETE o UPDATE</a></li><li><strong>una vez por cada fila afectada </strong>por las sentencias anteriores</li></ul>

<h3><strong>Crear un trigger</strong></h3>
<p>La sintaxis para crear un trigger es la siguiente:</p>
<pre>
CREATE TRIGGER nombre_trigger {BEFORE/AFTER} {INSERT/UPDATE/DELETE}
ON tabla &#91; FOR &#91; EACH ] {ROW/STATEMENT} ]
EXECUTE PROCEDURE función(argumentos);
</pre>
<p>CREATE TRIGGER es la sentencia para crear triggers y asignarles un nombre</p>
<p>BEFORE y AFTER indican si el trigger se ejecutará antes de o después de un INSERT, UPDATE o DELETE.</p>
<p>ON es para definir la tabla a la que se aplicará el trigger</p>
<p>FOR EACH indica que se ejecutará por cada fila ROW afectada o solo una vez junto a la sentencia STATEMENT</p>
<p>Tras EXECUTE PROCEDURE se referenciará la función que se ejecutará</p>

<p>Hay que tener en cuenta que la <strong>función</strong> que se va a ejecutar:</p>
<ul><li>deberá estar definida antes de crear el trigger</li><li>debe devolver el tipo trigger</li><li>no puede tener argumentos</li><li>puede usarse en varios triggers</li><li>puede desencadenar otros triggers</li></ul>
<p>Si esta <a href="https://programapa.wordpress.com/2020/10/27/funciones/">función </a>la definimos como PL/pgSQL podremos usar las<strong> variables especiales</strong> OLD y NEW para triggers de filas (ROW)</p>
<ul><li><strong>OLD</strong> contiene la fila que había antes de usar sobre ella UPDATE o DELETE </li><li><strong>NEW</strong> contiene la fila resultado de aplicar un INSERT o UPDATE</li></ul>

<h3><strong>Ejemplo: crear función y usarla en un trigger</strong></h3>
<p>A continuación vamos a crear un trigger que se active cada vez que se vaya a borrar una fila para que en su lugar se cambien los valores a NULL:</p>

<h4><strong>1- Crear la función</strong></h4>
<pre>
CREATE OR REPLACE FUNCTION no_borrar() RETURNS TRIGGER AS $no_borrar$
DECLARE plpgsql
BEGIN
RETURN NULL;
END;
$no_borrar$
LANGUAGE plpgsql;
</pre>
<p>no_borrar() es el nombre de la función</p>
<p>Con RETURNS TRIGGER indicamos que la función se usará en un trigger</p>
<p>AS es un requisito del lenguaje para indicar de nuevo el nombre de la función entre los símbolos $ tanto al principio como al final de la función.</p>
<p>Entre BEGIN y END se encuentra el código o expresión que se ejecutará. RETURN es un valor que siempre se devolverá al usar la función. También podría usarse un UPDATE o un INSERT por ejemplo.</p>
<p>En LANGUAGE indicamos que se usa Pl/pgSQL</p>

<h4><strong>2- Crear el trigger</strong></h4>
<pre>
CREATE TRIGGER trigger_borrado BEFORE DELETE
ON municipios FOR EACH ROW
EXECUTE PROCEDURE no_borrar();
</pre>
<p>Se crea un trigger llamado <em>trigger_borrado</em> que se ejecuta antes de usar DELETE sobre una tabla llamada <em>municipios</em>.</p>
<p>El trigger ejecutará la función no_borrar() creada anteriormente por cada fila afectada por DELETE, rellenándolas con valores nulos en vez de eliminarlas.</p>

<p><a id="borrar_trigger"></a></p>
<h3><strong>Borrar un trigger</strong></h3>
<p>La sintaxis para borrar un trigger es la siguiente:</p>
<pre>
DROP TRIGGER nombre_trigger ON tabla;
</pre>
  
</details>
  
## DML - Lenguaje de manipulación de datos´🛠
  
<details>
  <summary><strong>SELECT</strong></summary><br>
  
  <p>Los resultados de SELECT consisten en nuevas tablas que contienen los registros que se ajustan a nuestra consulta: parámetros o condiciones que hemos establecido en una sentencia. Esta tabla deberá guardarse si queremos conservarla.</p>
<p>A continuación se muestran varios ejemplos de la función SELECT en los que se va añadiendo el uso de cláusulas básicas como WHERE o DISTINCT. Todas pueden combinarse entre ellas para afinar los resultados y generar búsquedas más complejas.</p>

<h4><strong>Seleccionar columnas por su nombre</strong></h4>
<pre>
SELECT columna1, columna2
 FROM tabla;
</pre>

<h4><strong>Seleccionar todas las columnas de una tabla</strong></h4>
<pre>
SELECT * FROM tabla;
</pre>

<p>Seleccionar columnas de una tabla específica creando una nueva columna llamada columna3 con los resultados de sumar 50 a una de esas columnas.</p>
<pre>
SELECT columna1, (columna2 + 50) AS columna3
FROM tabla;
</pre>
<p>Pueden crearse expresiones usando <a href="https://programapa.wordpress.com/2020/10/01/operadores-en-postgresql/">operadores</a> para generar nuevas columnas a partir de los datos existentes. </p>
<p>Deben ir entre paréntesis.</p>

<h4><strong>No mostrar valores repetidos al seleccionar columnas con DISTINCT</strong></h4>
<pre>
SELECT DISTINCT columna1, columna2
 FROM tabla;
</pre>

<h4><strong>Filtrar la información mediante expresiones con WHERE </strong></h4>
<pre>
SELECT * FROM tabla 
WHERE columna1 &lt; 100;
</pre>
<p>Deben usarse <a href="https://programapa.wordpress.com/2020/10/01/operadores-en-postgresql/">operadores</a> </p>
<p>Este ejemplo selecciona los registros de una tabla cuando en la <em>columna1 </em>el valor es inferior a 100.</p>

<h4><strong>Ordenar una selección de datos con ORDER BY</strong></h4>
<pre>
SELECT columna1, columna2
FROM tabla
ORDER BY columna2 &#91;DESC];
</pre>
<p>Si no usamos DESC se ordenan de menor a mayor</p>
<p>Si usamos DESC se ordenan de mayor a menor</p>
<p>Admite números y texto</p>
<pre>
SELECT columna1, columna2
FROM tabla
ORDER BY columna2, columna1;
</pre>
<p>En caso de existir registros con el mismo valor en la columna 2 se ordenarían según la columna 1 y sucesivamente.</p>

<h4><strong>Mostrar los x primeros resultados de una selección con LIMIT</strong></h4>
<pre>
SELECT * FROM tabla
LIMIT 100;
</pre>
<p>En este caso se mostrarían solo los 100 primeros registros</p>

<h4><strong>Obviar los primeros x resultados de una selección con OFFSET</strong></h4>
<pre>
SELECT * FROM tabla
OFFSET 50;
</pre>
<p>En este caso no se mostrarían los 50 primeros registros</p>
<pre>
SELECT * FROM tabla
LIMIT 60
OFFSET 30
</pre>
<p>Combinando LIMIT y OFFEST para este caso se seleccionarán los registros 31 al 91.</p>

<h4><strong>Orden de la cláusulas</strong></h4>
<p>El<strong> orden </strong>en el que se deben escribir las cláusulas básicas SELECT dentro de una misma sentencia es el siguiente:</p>
<ol><li>SELECT</li><li>FROM</li><li>WHERE</li><li>ORDER</li><li>LIMIT</li><li>OFFSET</li></ol>
  
</details>
<details>
  <summary><strong>INSERT</strong></summary><br>
  
  <p>Con INSERT se introduce nuevas filas en la base de datos especificando los datos nuevos y las columnas en las que se colocarán.</p>
<p>Debe prestarse especial atención a la sintaxis de los datos que introducimos: el texto debe ir entrecomillado, al igual que las fechas. Los números no deben entrecomillarse, pues los tomará como texto. </p>
<p>Si los valores que introducimos se asignan a columnas que no soportan su tipo de datos (introducimos texto en una columna para números) se generará error.</p>
<pre>
INSERT INTO tabla &#91;(columna1, columna2, columna3...)]
VALUES (valor_columna1,valor_columna2,valor_columna3...);
</pre>
<p><strong>INSERT INTO</strong> especifica el lugar donde se añadirán los nuevos registros</p>
<p>Con <strong>VALUES</strong> asignamos valores en el orden que hemos especificado con INSERT INTO</p>
<p>Si no se han especificado columnas los valores tomarán la posición original de las columnas de la tabla en la que introducimos los datos.</p>

<h4><strong>Ejemplos de inserciones</strong></h4>
<pre>
INSERT INTO parcelas (tamaño, provincia, precio_m2)
VALUES (200,'Barcelona',80);
</pre>
<p>Solo se introducen datos en las columnas indicadas, por lo que si alguna otra columna no acepta datos nulos se generará error</p>

<pre>
INSERT INTO parcelas 
VALUES ('54548GL',200,'Barcelona',80);
</pre>
<p>En este caso se introducen datos en todas las columnas según su orden</p>
<p>El primer dato sería un identificador para la parcela de <a href="https://programapa.wordpress.com/2020/10/01/tipos-de-datos-en-sql/">tipo char(7)</a> por lo que debe ir entrecomillado</p>

<pre>
INSERT INTO parcelas
VALUES ('54548GL',200,'Barcelona',80),
       ('65882GL',520,'Tarragona',NULL);
</pre>
<p>Puede introducirse varios registros a la vez</p>
<p>Si no se quiere introducir datos para alguna columna en un registro concreto y la columna lo permite se puede especificar con NULL</p>
  
</details>
<details>
  <summary><strong>DELETE</strong></summary><br>
  
  <p>DELETE selecciona columnas al igual que SELECT pero lo que hace es borrarlas.</p>
<pre>
DELETE FROM tabla
</pre>
<ul><li>Elimina toda la tabla</li></ul>
<p>Podemos añadir cláusulas como WHERE para eliminar selectivamente los datos:</p>
<pre>
DELETE parcelas WHERE tamaño_m2 &lt; 1000 AND provincia = 'Salamanca'
</pre>
<ul><li>Elimina los registros de la tabla parcelas aquellas parcelas de Salamanca que tengan menos de 1000 metros cuadrados.</li></ul>
  
</details>
<details>
  <summary><strong>UPDATE</strong></summary><br>
  
  <p>UPDATE actualiza el valor de los registros seleccionados con nuevos valores.</p>
<p>La sintaxis básica de<strong> UPDATE </strong>es seleccionar una tabla y definir una expresión después de<strong> SET </strong>indicando columnas a actualizar y los nuevos valores que tendrán:</p>
<pre>
UPDATE tabla SET &#91;expresión]
</pre>

<h4><strong>Ejemplos de actualización de registros</strong></h4>
<pre>
UPDATE parcelas
SET valor_m2 = 5
WHERE tipo = 'rural' AND provincia = 'Córdoba'
</pre>
<p>Actualiza el valor del m2 a 5€ cuando la parcela se encuentra en la provincia de Córdoba y está catalogada como rural</p>

<pre>
UPDATE parcelas SET valor_m2 = valor_m2 * 1.03
</pre>
<p>Aumenta el valor del m2 de las parcelas un 3% multiplicando su valor por 1.03</p>
  
</details>
<details>
  <summary><strong>Consultar y agregar atributos</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/postgre_consultas-2.png?w=243" srcset="https://programapa.files.wordpress.com/2020/10/postgre_consultas-2.png 243w, https://programapa.files.wordpress.com/2020/10/postgre_consultas-2.png?w=150 150w" sizes="(max-width: 243px) 100vw, 243px" /></figure>
<p>En <strong>PostgreSQL</strong> las sentencias SELECT se utilizan para extraer información de la base de datos y generar tablas nuevas. Es parte de lo que se conoce como DML o Data Manipulation Language que introducimos en el post sobre<em> <a rel="noreferrer noopener" href="https://programapa.wordpress.com/2020/10/05/lista-de-instrucciones-basicas-en-postgresql-dml/" target="_blank">Select, Insert, Delete y Update</a>.</em> </p>
<p>Aquí veremos <strong>cláusulas específicas de SELECT para agregar datos</strong>. Son <a href="https://programapa.wordpress.com/2020/10/27/funciones/">funciones</a> que generan resultados únicos a partir de los valores que se encuentren en filas distintas, haciendo operaciones matemáticas sobre ellos o agrupándolos.</p>
<p>A continuación tenéis ejemplos de código SQL describiendo las cláusulas de agregación de datos empleadas con una hipotética tabla llamada  <em>tabla_arbolado</em> que cuenta con las columnas id, especie, altura_m, podas y nidos_aves.</p>

<h3><strong>Ejemplos de agregación de atributos</strong></h3>
<pre>
SELECT especie FROM tabla_arbolado GROUP BY especie;
</pre>
<p><strong>GROUP BY</strong> genera una nueva columna que agrupa todas las filas en valores únicos.</p>
<p>En este caso se han agrupado todos los árboles inventariados de un parque según su especie. A diferencia de DISTINCT, cada grupo (especie) se encuentra unido a los registros que aglutina (árboles), permitiendo operar con ellos para obtener estadísticas.</p>

<pre>
SELECT especie FROM tabla_arbolado GROUP BY especie,
HAVING altura_m &gt; 10;
</pre>
<p><strong>HAVING </strong>filtra los resultados de los grupos creados con GOUP BY.</p>
<p>Siguiendo con el ejemplo anterior, el resultado final será la agrupación de árboles por especie que midan más de 10 metros.</p>

<pre>
SELECT AVG(altura_m) FROM tabla_arbolado;
</pre>
<p><strong>AVG</strong> devuelve la media de los valores numéricos de la columna indicada.</p>
<p>En este caso obtendríamos la altura media de todos los árboles de la tabla arbolado.</p>

<pre>
SELECT COUNT(id_arbol) FROM tabla_arbolado';
SELECT COUNT(id_arbol) AS recuento_acacias FROM tabla_arbolado WHERE especie = 'acacia';
</pre>
<p><strong>COUNT</strong> devuelve el número de filas que cumplen la condición indicada.</p>
<p>En el primer caso nos diría cuántos árboles hay en el parque.</p>
<p>En el segundo caso obtendríamos el número de acacias del parque.</p>

<pre>
SELECT MAX(altura_m) FROM tabla_arbolado;
</pre>
<p><strong>MAX</strong> devuelve el valor máximo de la columna seleccionada.</p>
<p>En este caso nos diría cuántos metros mide el árbol más alto del parque.</p>

<pre>
SELECT MIN(podas) FROM tabla_arbolado;
</pre>
<p><strong>MIN</strong> devuelve el valor mínimo de la columna seleccionada.</p>
<p>En este caso nos diría cuál es el menor número de podas que ha tenido algún árbol en el parque.</p>

<pre>
SELECT SUM(nidos_aves) FROM tabla_arbolado;
</pre>
<p><strong>SUM</strong> devuelve la suma de los valores numéricos de una columna seleccionada.</p>
<p>En este caso nos diría el número total de nidos de pájaro que se han descubierto en el parque.</p>

<pre>
SELECT ARRAY_AGG(especie) FROM tabla_arbolado;
</pre>
<p><strong>ARRAY_AGG</strong> crea una lista con los valores de la columna indicada, incluyendo los nulos.</p>
<p>En este ejemplo obtendríamos una lista con todas las especies existentes en el arbolado del parque.</p>

<p>Las cláusulas anteriores son combinables entre si, pudiendo hacer <strong>selecciones más complejas</strong>:</p>
<pre>
SELECT especie, COUNT(nidos_aves) AS recuento_nidos, 
COUNT(podas) AS recuento_podas FROM tabla_arbolado 
GROUP BY nombre 
ORDER BY recuento_nidos DESC;
</pre>
<p>Con este código obtendríamos el número de nidos de ave y de podas que se han encontrado en cada una de las especies, ordenado de mayor a menor número de nidos.</p>
  
</details>
<details>
  <summary><strong>JOIN - Unir tablas </strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/join.png?w=300" srcset="https://programapa.files.wordpress.com/2020/10/join.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/join.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/join.png 495w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>Las bases de datos en <strong>PostgreSQL </strong>suelen estar formadas por varias tablas como consecuencia del diseño lógico en el proceso inicial de su creación.</p>
<p>En ocasiones necesitamos que la información dispersa en varias tablas se muestre unida en nuevas tablas, permitiendo ver la relación entre los datos y operar con ellos fácilmente. Para realizar esto en PostgreSQL se utilizan las sentencias JOIN.</p>

<h3><strong>CROSS JOIN</strong></h3>
<p>Esta sentencia une cada fila de la primera tabla con cada fila de la segunda tabla. Generará por tanto una nueva tabla con el número de filas resultado de multiplicar el número de filas de la tabla 1 por el número de filas de la tabla 2:</p>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/crossjoin.png?w=526" width="319" height="332" srcset="https://programapa.files.wordpress.com/2020/10/crossjoin.png?w=319 319w, https://programapa.files.wordpress.com/2020/10/crossjoin.png?w=144 144w, https://programapa.files.wordpress.com/2020/10/crossjoin.png?w=288 288w, https://programapa.files.wordpress.com/2020/10/crossjoin.png 526w" sizes="(max-width: 319px) 100vw, 319px" /></figure>
<p>Si tuviésemos dos tablas, una con las provincias de España y otra con una lista de categorías de usos del suelo, obtendríamos una nueva tabla en la que cada fila correspondería a una provincia y un uso del suelo.</p>
<p>Las columnas que contendría la nueva tabla se especifica en SELECT. En el siguiente caso se añadirían las columnas Nombre y Uso:</p>
<pre>
SELECT provincias.Nombre, usos_del_suelo.Uso 
FROM provincias, usos_del_suelo;
</pre>
<pre>
SELECT provincias.Nombre, usos_del_suelo.Uso
FROM provincias
CROSS JOIN usos_del_suelo;
</pre>
<p>Resultado:</p>
<figure><table><thead><tr><th>Nombre</th><th>Uso</th></tr></thead><tbody><tr><td>Almería</td><td>Cultivo</td></tr><tr><td>Almería</td><td>Industrial</td></tr><tr><td>Almería</td><td>Forestal</td></tr><tr><td>Albacete</td><td>Cultivo</td></tr><tr><td>Albacete</td><td>Industrial</td></tr><tr><td>Albacete</td><td>Forestal</td></tr></tbody></table></figure>

<h3><strong>INNER JOIN</strong></h3>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/inner-join.png?w=337" width="182" height="127" srcset="https://programapa.files.wordpress.com/2020/10/inner-join.png?w=182 182w, https://programapa.files.wordpress.com/2020/10/inner-join.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/inner-join.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/inner-join.png 337w" sizes="(max-width: 182px) 100vw, 182px" /></figure>
<p>Con INNER JOIN lo que hacemos es unir en una misma fila aquellos registros de las tablas seleccionadas que compartan un atributo en común.</p>
<p>Pongamos que tenemos dos tablas, una con las provincias y otra con municipios. La de provincias tiene una columna ID con un identificador único para cada una y la de municipios tiene una columna con el código de la provincia en la que están. </p>
<p>Al hacer el INNER JOIN obtendríamos una nueva tabla en la que cada fila almacenaría tanto el nombre de la provincia como el del municipio. Debe indicarse qué columna de la primera tabla corresponde a la de la segunda, pudiendo hacer uso de abreviaturas (j y k para este ejemplo).</p>
<p>Las columnas que contendría la nueva tabla se especifica en SELECT. En el siguiente caso se añadirían todas las columnas al usar el asterisco (*):</p>
<pre>
SELECT * FROM provincias j 
INNER JOIN municipios k
ON j.ID = k.ID_prov;
</pre>
<p>INNER JOIN es el JOIN por defecto, por lo que puede prescindirse de escribir JOIN</p>
<pre>
SELECT * FROM provincias j 
JOIN municipios k
ON j.ID = k.ID_prov;
</pre>
<p>Resultado:</p>
<figure><table><thead><tr><th>ID</th><th>Prov</th><th>ID_Prov</th><th>Municipio</th></tr></thead><tbody><tr><td>1</td><td>Almería</td><td>1</td><td>Níjar</td></tr><tr><td>1</td><td>Almería</td><td>1</td><td>Gádor</td></tr><tr><td>1</td><td>Almería</td><td>1</td><td>Aguadulce</td></tr><tr><td>2</td><td>Albacete</td><td>2</td><td>Tinajeros</td></tr><tr><td>2</td><td>Albacete</td><td>2</td><td>Barrax</td></tr><tr><td>2</td><td>Albacete</td><td>2</td><td>Argamasón</td></tr></tbody></table></figure>

<h3><strong>OUTER JOIN</strong></h3>
<p>Une las tablas incluyendo las filas que no estén unidas por campos en común. Ello hace que se generen campos nulos, pero permite ver si hay registros en una tabla que no guardan la relación que deberían con la otra. </p>
<p>Hay tres tipos según qué tabla queremos que una todas sus filas:</p>
<h4><strong>LEFT OUTER JOIN </strong></h4>
<div><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/left-join.png?w=337" width="198" height="138" srcset="https://programapa.files.wordpress.com/2020/10/left-join.png?w=198 198w, https://programapa.files.wordpress.com/2020/10/left-join.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/left-join.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/left-join.png 337w" sizes="(max-width: 198px) 100vw, 198px" /></figure>
<p>Genera una nueva tabla uniendo todas las filas de la primera tabla, aunque muchas de ellas no guarden relación con un campo en común con la otra.</p>
<p>Por ejemplo, si tenemos una primera tabla con las provincias de España y su ID, y otra con las inundaciones de España durante 2019 con el ID de la provincia en el que se originaron, al hacer el LEFT JOIN obtendríamos una tabla en la que conservaríamos las provincias en las que no se han producido inundaciones:</p>
<pre>
SELECT * FROM provincias j 
LEFT &#91;OUTER] JOIN inundacion_2019 k
ON j.ID = k.ID_prov;
</pre>
<ul><li>No es necesario escribir el OUTER</li><li>Las columnas que contendría la nueva tabla se especifica en SELECT. En este caso se añadirían todas las columnas al usar el asterisco (*)</li></ul>
<p>Resultado:</p>
<figure><table><thead><tr><th>ID</th><th>Prov</th><th>ID_Prov</th><th>Inundado</th></tr></thead><tbody><tr><td>1</td><td>Almería</td><td>1</td><td>Níjar</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>San Javier</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Totana</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Torre Pacheco</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>San Miguel de Salinas</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>Pilar de la Horadada</td></tr><tr><td>4</td><td>Las Palmas</td><td>NULL</td><td>NULL</td></tr><tr><td>5</td><td>Santa Cruz</td><td>NULL</td><td>NULL</td></tr></tbody></table></figure>

<h4><strong>RIGHT OUTER JOIN </strong></h4>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/right-join.png?w=337" width="209" height="146" srcset="https://programapa.files.wordpress.com/2020/10/right-join.png?w=209 209w, https://programapa.files.wordpress.com/2020/10/right-join.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/right-join.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/right-join.png 337w" sizes="(max-width: 209px) 100vw, 209px" /></figure>
<p>Hace lo mismo pero son las filas de la segunda tabla las que son todas unidas a las de la primera.</p>
<p>Usando el ejemplo anterior, si hiciésemos RIGHT JOIN solo obtendríamos de la tabla de provincias aquellas en las que se han producido inundaciones, conservando todos los registros sobre inundaciones (incluiría así a Ceuta y Melilla, que son Ciudades Autónomas):</p>
<pre>
SELECT * FROM provincias j 
RIGHT &#91;OUTER] JOIN inundacion_2019 k
ON j.ID = k.ID_prov;
</pre>
<ul><li>No es necesario escribir el OUTER</li><li>Las columnas que contendría la nueva tabla se especifica en SELECT. En este caso se añadirían todas las columnas al usar el asterisco (*)</li></ul>
<p>Resultado:</p>
<figure><table><thead><tr><th>ID</th><th>Prov</th><th>ID_Prov</th><th>Inundado</th></tr></thead><tbody><tr><td>1</td><td>Almería</td><td>1</td><td>Níjar</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>San Javier</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Totana</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Torre Pacheco</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>San Miguel de Salinas</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>Pilar de la Horadada</td></tr><tr><td>NULL</td><td>NULL</td><td>0</td><td>Ceuta</td></tr><tr><td>NULL</td><td>NULL</td><td>0</td><td>Melilla</td></tr></tbody></table></figure>

<h4><strong>FULL OUTER JOIN</strong></h4>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/full-join.png?w=337" alt="" class="wp-image-1844" width="219" height="153" srcset="https://programapa.files.wordpress.com/2020/10/full-join.png?w=219 219w, https://programapa.files.wordpress.com/2020/10/full-join.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/full-join.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/full-join.png 337w" sizes="(max-width: 219px) 100vw, 219px" /></figure>
<p> Crea una nueva tabla con todas las filas de las tablas a unir:</p>
<pre>
SELECT * FROM provincias j 
FULL &#91;OUTER] JOIN inundacion_2019 k
ON j.ID = k.ID_prov;
</pre>
<ul><li>No es necesario escribir el OUTER</li><li>Las columnas que contendría la nueva tabla se especifica en SELECT. En este caso se añadirían todas las columnas al usar el asterisco (*)</li></ul>
<p>Resultado:</p>
<figure><table><thead><tr><th>ID</th><th>Prov</th><th>ID_Prov</th><th>Inundado</th></tr></thead><tbody><tr><td>1</td><td>Almería</td><td>1</td><td>Níjar</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>San Javier</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Totana</td></tr><tr><td>2</td><td>Murcia</td><td>2</td><td>Torre Pacheco</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>San Miguel de Salinas</td></tr><tr><td>3</td><td>Alicante</td><td>3</td><td>Pilar de la Horadada</td></tr><tr><td>4</td><td>Las Palmas</td><td>NULL</td><td>NULL</td></tr><tr><td>5</td><td>Santa Cruz</td><td>NULL</td><td>NULL</td></tr><tr><td>NULL</td><td>NULL</td><td>0</td><td>Ceuta</td></tr><tr><td>NULL</td><td>NULL</td><td>0</td><td>Melilla</td></tr></tbody></table></figure>

<h4><strong>JOINS MÚLTIPLES</strong></h4>
<div ><figure ><img loading="lazy" src="https://programapa.files.wordpress.com/2020/10/join-multiples.png?w=562" alt="" class="wp-image-1846" width="353" height="155" srcset="https://programapa.files.wordpress.com/2020/10/join-multiples.png?w=353 353w, https://programapa.files.wordpress.com/2020/10/join-multiples.png?w=150 150w, https://programapa.files.wordpress.com/2020/10/join-multiples.png?w=300 300w, https://programapa.files.wordpress.com/2020/10/join-multiples.png 562w" sizes="(max-width: 353px) 100vw, 353px" /></figure>
<p>Para unir más de dos tablas deben encadenarse un JOIN con otro. El primer JOIN une la primera y la segunda tabla. El segundo une ese resultado con una tercera tabla, el tercero unirá ese resultado con una cuarta tabla y así sucesivamente.</p>
<p>Como ejemplo, podríamos unir al FULL JOIN del ejemplo anterior una tercera tabla con la lista de municipios que han recibido ayudas para la reparación de los daños:</p>
<pre>
SELECT * 
FROM provincias j 
FULL &#91;OUTER] JOIN inundacion_2019 k
ON j.ID = k.ID_prov
LEFT JOIN ayudas f
ON k.Inundado = f.Ayuda;
</pre>
<p>Resultado:</p>
  
</details>
<details>
  <summary><strong>Subconsultas</strong></summary><br>
  
  <div><figure ><img src="https://programapa.files.wordpress.com/2020/10/subconsultas.jpg?w=300" srcset="https://programapa.files.wordpress.com/2020/10/subconsultas.jpg?w=300 300w, https://programapa.files.wordpress.com/2020/10/subconsultas.jpg?w=600 600w, https://programapa.files.wordpress.com/2020/10/subconsultas.jpg?w=150 150w" sizes="(max-width: 300px) 100vw, 300px" /></figure>
<p>En ocasiones necesitamos hacer consultas en <strong>PostgreSQL</strong> que por su complejidad nos obligan a escribir códigos largos, teniendo que dividirlo en varias partes según el procedimiento lógico que nos haga llegar al resultado que esperamos.</p>
<p>En vez de encadenar instrucciones dependientes unas de otras se puede <strong>simplificar</strong> el proceso anidando una consulta dentro de otra, es decir, creando <strong>subconsultas</strong>.</p>
<p>Las subconsultas son sentencias SELECT metidas entre paréntesis () dentro de otras <a href="https://programapa.wordpress.com/2020/10/05/lista-de-instrucciones-basicas-en-postgresql-dml/">sentencias SELECT, INSERT, DELETE y UPDATE</a>. </p>
<p>A continuación tienes los <strong>distintos tipos de subconsultas</strong> que existen. Les acompañarán ejemplos tomando como base una tabla ficticia en la que se almacenan municipios con su nombre, superficie, altitud media, población y provincia a la que pertenecen.</p>
<p>El <a href="https://programapa.wordpress.com/2020/10/01/operadores-en-postgresql/">operador</a> siempre debe escogerse con cuidado dependiendo del resultado que queramos obtener con la subconsulta. Si el resultado esperado es un único valor o registro usaremos los operadores aritméticos o de comparación, pero si vamos a obtener varios valores habrá que echar mano de los operadores lógicos.</p>

<h3><strong>Subconsulta como expresión</strong></h3>
<p>Selecciona datos a partir de otra selección.</p>
<pre>
SELECT columna 
FROM tabla 
WHERE columna OPERADOR (subconsulta);
</pre>

<h4><strong>Ejemplos</strong></h4>
<pre>
SELECT nombre
FROM municipios
WHERE superficie &gt; ALL (SELECT superficie FROM municipios
WHERE provincia = 'León' OR población &gt; 10000 );
</pre>
<p>Listaríamos los municipios cuya superficie es mayor que la superficie de cualquier municipio de la provincia de León o que su población supere los 10000 habitantes.</p>

<pre>
SELECT nombre
FROM municipios
WHERE altitud &lt; ANY (SELECT altitud FROM municipios
WHERE provincia = 'Huelva');
</pre>
<p>Seleccionaríamos los municipios cuya altitud media sea inferior a la que tenga cualquier municipio de la provincia de Huelva.</p>

<pre>
SELECT nombre
FROM municipios
WHERE provincia IN (SELECT provincia FROM municipios
GROUP BY provincia 
ORDER BY SUM(población) DESC
LIMIT 5);
</pre>
<p>Averiguaríamos cuáles son los municipios de las 5 provincias más pobladas.</p>
<p>IN compara la provincia a la que pertenece cada municipio con las 5 provincias que obtienen un valor más alto al sumar los valores de población de sus municipios.</p>

<h3><strong>Subconsultas como tabla</strong></h3>
<p>En este caso se usa una subconsulta para filtrar la fuente de información sobre la que se hará la consulta principal. Para ello, se usa justo después de FROM.</p>
<pre>
SELECT columna 
FROM (subconsulta_tabla1)
JOIN tabla2;
</pre>
<p>Este tipo de subconsulta resulta útil, por ejemplo, cuando vas a realizar un <a href="https://programapa.wordpress.com/2020/10/19/combinar-registros-de-tablas-diferentes-en-postgresql-con-join/">JOIN</a>, ya que así los datos se procesarían más rápido.</p>

<h4><strong>Ejemplo</strong></h4>
<pre>
SELECT nombre FROM (SELECT nombre, población FROM municipios) 
WHERE población &gt; 5000 
</pre>
<p>Hemos seleccionado los municipios con más de 5000 habitantes cargando solo las dos columna que íbamos a usar: nombre y población.</p>

<h3><strong>Subconsultas correlacionadas</strong></h3>
<pre>
SELECT (subconsulta1)(subconsulta2)
FROM tabla
WHERE columna OPERADOR (expresión);
SELECT columna
FROM tabla
WHERE columna OPERADOR (subconsulta con función_agregado);
</pre>
<p>Una de las consultas o subconsultas sobre una tabla dependen del resultado de otra consulta sobre esa misma tabla.</p>
<p>En el primer caso lo establecido en la subconsulta 1 sería necesario para llevar a cabo la subconsulta 2.</p>
<p>En el segundo caso la subconsulta contendría una función de agregado cuyo resultado alimentaría a la consulta principal.</p>

<h4><strong>Ejemplo</strong></h4>
<pre>
SELECT nombre 
FROM municipios
WHERE altitud &gt; (SELECT AVG(altitud) FROM municipios);
</pre>
<p>Obtendríamos los nombres de los municipios cuya altitud supera a la media de altitud de todos los municipios.</p>
<p>La altitud media es un dato que no conocemos pero que tampoco ha sido necesario que lo calculáramos previamente porque se ha utilizado la función de agregado AVG (calcula la media aritmética)</p>
  
</details>
  
## PostGIS 🐘🌏

<details>
  <summary><strong>Datos geométricos</strong></summary><br>
  
  </details>
  
<details>
  <summary><strong>Predicados espaciales</strong></summary><br>
  
  </details>
  
<details>
  <summary><strong>Crear topología</strong></summary><br>
  
  </details>
  
<details>
  <summary><strong>Crear topología de red</strong></summary><br>
  
  </details>
  
  <details>
  <summary><strong>Importar red viaria OSM</strong></summary><br>
  
  </details>

## ¡Sígueme!
[![](https://img.shields.io/badge/@progra_mapa-white?style=for-the-badge&labelColor=blue&logo=Twitter&logoColor=white)](https://twitter.com/progra_mapa)[![](https://img.shields.io/badge/PrograMapa-grey?style=for-the-badge&logo=wordpress)](https://programapa.wordpress.com)[![](https://img.shields.io/badge/Roberto-blue?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/robertojl)[![](https://img.shields.io/badge/@progra_mapa-white?style=for-the-badge&logo=instagram)](https://instagram.com/progra_mapa)
