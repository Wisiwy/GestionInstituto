# CRUD en Laravel
Vamos a realizar un CRUD para el modelo Alumnos.

> Crear (Create), Leer (Read), Actualizar (Update) y Eliminar (Delete). Estas acciones son esenciales para gestionar datos en aplicaciones y sistemas informáticos.

Laravel usa la libreria Eloquent ORM 
> ORM es un marco de trabajo o libreria que nos provee comunicacion entre una base de datos y un lenguaje de programacion.
> Eloquen permite intractuar con la BD. Crea la clase para crear CRUD

---
## 1. Generar ecosistema CRUD para Alumno
Comando para creación modelo Alumno:
``` bash 
php artisan make:model Alumno --all
```
<details><summary>Ayuda *make:model*</summary>

``` bash 
php artisan help make:model 
```
</details>

### Archivos creados
- **Modelo**: Clase que permite interactuar con una tabla concreta de la Base de Datos a través de nuestra aplicacion.
- **Controlador**: clase con los métodos que se ejecutarán dependiendo de la situación. 
- **Migrate**: Clase anónima que sirve para vrear o eliminar elementos DDL. Up or Down.
- **Factory**: Clase para fabricar el tipo de registros que metemos en la tabla. 
- **Seeder**: Puebla la BD con los valores fabricados con el modelo *Factory*.
---

## 2. Creacion tabla *Alumnos* y relacionarla con el *Modelo*


### 2.1 Crear ESQUEMA tabla Alumnos
En la generación del modelo  Alumnos se el archivo:
>    _**'nombreProyecdto'/database/migrations/_create_alumnos_table.phap**_

Este archivo tiene los metodos *up* y *down*. En el primero especificamos el esquema con la clase **Schema::create**.

<details>
  <summary> <span style="font-size:large; color: dodgerblue">Ver Codigo</span>  </summary>

```php
    public function up(): void
    {
        Schema::create('alumnos', function (Blueprint $table) {
            $table->id();
            $table->string("nombre");
            $table->string("email");
            $table->string("dir");
            $table->string("dni");
            $table->timestamps();
        });
    }
```
</details> <!-- Código esquema  -->

### 2.2 Desde MODELO llamar a la tabla creada. 
Especificar la tabla de la BD a la que esta asociada el modelo:
```php
class Alumno extends Model{
    use HasFactory;
    protected $table="alumnos";
}
```

## 3. Fabricar valores para rellenar tabla *Alumnos*
### 3.1 Modificar *AlumnoFactory*
Factory es la fábrica donde indicamos las reglas para generar nuestros nuevos registros como los construiremos.
//TODO terminar de pasar archivo de Drive. 
En este archivo se espedifin

### 3.2 Generar registros en *AlumnoSeeder*

1. Llamamos al seeder de Alumno desde *databaseSeeders*
```php
    $this->call([
        AlumnoSeeder::class,
        ProfesorSeeder::class    
    ])
}
```
2. Llamamos a al *factory* de Alumno y  especificamos cuantos registros: 
```php
class AlumnoSeeder extends Seeder{
    public function run (): void {
        Alumno::factory()->count(100);
    }
}
```

### Otros conceptos
- **Policy**: Define permisos. 
- **Request**: Validadcion de susuarios.
- **Routes**: Definier las redirecciones.

---
### Comandos interesantes

Recoger todos registros de una tabla (*select * from alumnos*). Por eloquent 
``` php
$alumnos = Alumno::all();
```
---
### API, SOAP y REST 
<details>
  <summary>Click  to expand </summary>

### API
    Representa un conjunto de reglas y herramientas que permiten la interacción entre diferentes software.
    Puede utilizar tanto REST como SOAP como protocolos de comunicación.
### REST (Transferencia de Estado Representacional)
      Se basa en un conjunto de principios arquitectónicos que utiliza el protocolo HTTP para realizar operaciones CRUD
    (Crear, Leer, Actualizar, Eliminar) en recursos. Especifica recomendaciones, como el uso de los métodos HTTP PUT 
    para actualizar todos los datos y PATCH para actualizar solo un tipo de datos en una solicitud al servidor.
### SOAP (Protocolo Simple de Acceso a Objeto)
       Es un protocolo de comunicación definido por estándares, como el W3C, que utiliza XML para la transmisión de 
    mensajes entre aplicaciones.
    A diferencia de REST, que utiliza principalmente HTTP, SOAP puede trabajar sobre otros protocolos de red.
</details>