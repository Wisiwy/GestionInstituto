# CRUD en Laravel
Vamos a realizar un CRUD para el modelo Alumnos.

> Crear (Create), Leer (Read), Actualizar (Update) y Eliminar (Delete). Estas acciones son esenciales para gestionar datos en aplicaciones y sistemas informáticos.

Laravel usa la libreria Eloquent ORM 
> ORM es un marco de trabajo o libreria que nos provee comunicacion entre una base de datos y un lenguaje de programacion.
> Eloquen permite intractuar con la BD. Crea la clase para crear CRUD

---
## 1. Generar ecosistema CRUD para Alumno
Comando para creación modelo Alumno (*model, factory, seed, migration*):
``` bash 
php artisan make:model Alumno --all
```
<details><summary>Ayuda *make:model* uy *make:seed*</summary>

#### Crear unicamente modelo
``` bash 
php artisan help make:model Alumno
```
#### Crear unicamente seed
``` bash 
php artisan make:seed Alumno
```

</details>

### Archivos creados
- *app*/**Models**: Clase que permite interactuar con una tabla concreta de la Base de Datos a través de nuestra aplicacion.
- **Controlador**: clase con los métodos que se ejecutarán dependiendo de la situación. 
- *database*/**Migration**: Clase anónima abstracta que sirve para crear o eliminar elementos DDL. Dos métodos: Up or Down. Ejecutar DDL. 
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
---
## 3. Fabricar valores para rellenar tabla *Alumnos*
### 3.1 Modificar *AlumnoFactory*
Factory es la fábrica donde indicamos las reglas para generar nuestros nuevos registros como los construiremos.
//TODO terminar de pasar archivo de Drive. 
En este clase se especifica como se construira los registros:
```php
public function definition(): array
    {
        return [
            "nombre"=>fake()->name(),
            "dir"=>fake()->address(),
            "email"=>fake()->email(),
            "dni"=>$this->dni()
        ];
    }
```
<details>
  <summary> <span style="font-size:large; color: dodgerblue">Funcion dni()</span>  </summary>

```php
 private function dni(){
        $letras = "TRWAGMYFPDXBNJZSQVHLCKE";
        $num_dni = fake()->randomNumber(8,true);
        $num_dni= $num_dni<0? -($num_dni): $num_dni;
        info("número generao $num_dni");
        $letra = $letras[$num_dni%23];
        $dni = "$num_dni-$letra";
        return $dni;
    }
```
</details>


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
        Alumno::factory()->count(100)->create();
    }
}
```
- *create()* &rarr; Inserta los valores de factory en la tabla *Alumno* de la BD. 
#### <span style = "color:green;"> Ejecución seeders por artisan </span>
```bash
php artisan db:seeder
```
 Ejecuta los seedes de *DatabaseSeeder.php* se llaman los seeders de los diferentes modelos.

---
## 4. Migración
>  Clase abstracta con dos métodos up y down (crear y elimniar). Sirve para ejecutar DDL

```bash
php artisan migrate:fresh --seed
```
<details>
  <summary> <span style="font-size:small ; color: #267700">Explicación comando</span>  </summary>

- ***fresh*** &rarr; Borra todo y vuelve a crear. 
- ***--seed*** &rarr; Puebla la base de datos


</details>





---

---

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