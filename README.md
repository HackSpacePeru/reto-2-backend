# Reto 2 de Backend

Para este reto las nuevas herramientas a entender son:

-Uso de update() y remove() con firebase.

-Uso de underscore.js


## Entrega del Reto 2

La entrega de este reto será una aplicación que funcione como phonebook que nos permita listar, registrar, actualizar y eliminar contactos que esten almacenados en nuestro firebase , el reto implica conocimientos de frontend para contruir su css, sugerimos utilizar bootstrap, pero la idea principal es utilizar javascript y firebase. 

El formato a enviar es un comprimido (.zip o .rar) con los archivos que impliquen tu api, se evaluará funcionalidad, más allá del css que se utilice.

![Pantallazo]()

## Crea tu Firebase

Si aún no lo has hecho, registrarse para obtener una cuenta. Luego de haberlo hecho agrega el url de tu firebase a tu app como ya te lo explicamos.

```js
var myFirebase = new Firebase('https://miFirebaseUrl.firebaseio.com/');
```

## Crea tu estructura de datos

En lenguaje javascript definiremos la estructura que se almacenara en formato JSON en nuestro firebase, ya que se trata de un phonebook, los datos primordiales serian el nombre, direccion, celular y el correo del contacto. Como se observa cada atributo de item a excepcion del id tiene asociado a un valor de un elemento html que nosotros escribiremos a la hora de hacer la vista html de nuestra app.


```js
var contacto = { 
          id: ran(),
          nombre: $("#nombre").val(), 
          direccion: $("#direccion").val(),  
          celular: $("#celular").val(),
          correo: $("#correo").val()
        };
var ran = function () {
				  return '_' + Math.random().toString(33).substr(3, 10);
				};
```

## Uso de update() y remove() con firebase

Ya sabemos como almacenar datos en nuestra base de datos en firebase. Ahora para poder actualizar y eliminar datos utilizamos update() y remove(), por ejemplo de la siguiente manera:

```js
//actualizamos un elemento con un respectivo id
actualizar: function(item){
        myFireBase.child("contacto").child(item.Id).update(item);
    }
//eliminamos un elemento con un respectivo id
eliminar: function(id){
        myFireBase.child("contacto").child(id).remove();
    }
```
### Control de errores

Dentro de nuestros metodos de actualizar y eliminar es recomendable incluir otro metodo que maneje errores al momento de conectarse y modificar elementos en la base de datos.

```js
var onComplete = function(error) {
          if (error)
            alert('Error en la modificación/eliminación.');
          else
            alert('Item modificado/eliminado.');
        }
```

## Renderizar un html para poder actualizar los datos en tiempo real

Ya que necesitamos mostrar los datos en tiempo real (actuales) que se encuentran almacenados en nuestra base de datos, utilizaremos un html renderizado, esto quiere decir que el html se contruira como texto y luego gracias a javascript se incrustara en el documento html. Para ello utilizaremos el metodo que ya conocemos en jquery, html().

```js
$("elemento al que se le inserta el html").html("<p>HTML a insertar</p>");
```
Por ejemplo, si tenemos un tabla estatica en html:

<table id="Tabla">
					<thead>
						<tr>
							<th>Nombre</th>
							<th>Dirección</th>
							<th>Celular</th>
							<th>Correo</th>
						</tr>
					</thead>
					<tbody>
						<!-- tbody llenado con js --> 
					</tbody>
</table>

Podemos agregar un html de la siguiente manera:

```js
renderHtml: function(){
        var html="";
        myFirebase.on('value', function(data) {
            var contactos = data.val();
            //A continuacion explicaremos el use de _.each
            _.each(contactos['contacto'], function(contacto){
                html+="<tr id='"+contacto.id+"' data-id='"+v.id+"'>";
                    html+="<td>"+contacto.nombre+"</td>";
                    html+="<td>"+contacto.direccion+"</td>";
                    html+="<td>"+contacto.celular+"</td>";
                    html+="<td>"+contacto.correo+"</td>";
                html+="</tr>";                
            });
            $('#Tabla > tbody').html(html);
            //Para esto no olvidar incluir la libreria de jquery
        });

    },
```

## Utilizando underscore.js

Utilizando la librería underscore.js y su metodo each(), nos permite repetir una funcion en una lista de elementos, iterando con cada uno. Puedes descargar la librería [aquí](http://underscorejs.org/).

Primero incluimos la librería en nuestro html.

```html
<script src="http://underscorejs.org/underscore-min.js">
```

Luego podemos utilizar su metodo each.

```js
_.each(lista, funcion(input))
```


