# Práctica 3.2. Creación de Pipes personalizados en Angular

## Objetivos
- Crear y utlizar Pipes personalizados en Angular.
- Aprender a transformar datos en tu aplicación de manera eficiente.

## Duración aproximada
- 30 minutos.

## Instrucciones

## Tipos de Data Binding

1. Interpolación:

    - Utilizar la sintaxis de doble llaves {{ }} para mostrar datos del componente en la vista.

````html
<h1>{{ titulo }}</h1>
````

2. Property Binding:

 - Permite enlazar propiedades del componente a atributos de elementos HTML utilizando corchetes [ ].

````html
<img [src]="urlImagen" [alt]="descripcion">
````

3. Event Binding:

 - Escuchar eventos del DOM y llamar a métodos en el componente utilizando paréntesis ( ).

````html
<button (click)="accion()">Click me</button>
````

4. Two-Way Data Binding:

 - Sincronizar datos de manera bidireccional entre el componente y la vista, usando ngModel.
 - Requiere importar FormsModule.

````html
<input [(ngModel)]="nombre">

````

## Atributos, Clases y Estilos Binding en Angular

1. Atributo Binding
 - El Atributo Binding permite enlazar propiedades de HTML a las propiedades del componente utilizando corchetes [ ]. Esto es útil para establecer atributos que pueden cambiar dinámicamente.


````html

<img [src]="imagenUrl" [alt]="descripcion">
````
-  En este caso, imagenUrl y descripcion son propiedades del componente que definen la URL y la descripción de la imagen.

2. Clases Binding
 - El Clases Binding se utiliza para agregar o quitar clases CSS de un elemento de manera dinámica. Usar la directiva ngClass para enlazar clases a condiciones en el componente.


````html

<div [ngClass]="{'activo': isActivo, 'inactivo': !isActivo}">Contenido</div>
````
 - Aquí, la clase activo se aplica si isActivo es verdadero, e inactivo si es falso.

3. Estilos Binding
 - El Estilos Binding permite aplicar estilos CSS directamente a un elemento HTML usando la directiva ngStyle. Esto es útil para cambiar estilos basados en condiciones.


````html

<div [ngStyle]="{ 'background-color': color, 'font-size': tamano + 'px' }">Texto</div>
````
 - En este caso, color y tamaño son propiedades del componente que determinan el color de fondo y el tamaño de fuente del elemento.



## Pipes en Angular

1. Transformación de Datos:

 - Los Pipes permiten aplicar transformaciones a datos en tiempo real, mejorando la presentación de la información.

2. Pipes Integrados:

- Angular ofrece varios Pipes incorporados, como:
    - DatePipe: para formatear fechas.
    - CurrencyPipe: para mostrar valores monetarios.
    - DecimalPipe: para formatear números.
    
3. Pipes Personalizados:

 - Puedes crear tus propios Pipes personalizados para realizar transformaciones específicas que no están cubiertas por los Pipes integrados.


4. Uso en Plantillas:

 - Los Pipes se aplican utilizando la sintaxis {{ valor | pipeName }} en las plantillas HTML.

## Ejemplo de Uso de Pipes

1. Uso de un Pipe Integrado:

````html

<p>Fecha: {{ fecha | date:'fullDate' }}</p>

````

2. Creación de un Pipe personalizado: Para crear un Pipe que invierte una cadena de texto, seguir estos pasos:

 - Generar el Pipe:

````bash

ng generate pipe invertirTexto

````

 - Implementar el Pipe:

````typescript

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'invertirTexto'
})
export class InvertirTextoPipe implements PipeTransform {
  transform(value: string): string {
    return value.split('').reverse().join('');
  }
}

````

 - Usar el Pipe en una plantilla:

````html

<p>{{ 'Angular' | invertirTexto }}</p> <!-- Salida: "ralugnA" -->

````

## Pasos para crear un Pipe personalizado

1. Generar el Pipe: Usar Angular CLI para generar un nuevo Pipe:

````bash

ng generate pipe nombre-del-pipe

````

2. Implementar el Pipe: Editar el archivo del Pipe generado (por ejemplo, nombre-del-pipe.pipe.ts) para implementar la lógica de transformación:

````typescript

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'nombreDelPipe'
})
export class NombreDelPipe implements PipeTransform {
  transform(value: string, ...args: any[]): string {
    // Lógica de transformación
    return value.toUpperCase(); // Ejemplo: convertir a mayúsculas
  }
}

````

3. Usar el Pipe en el Template: Una vez creado, puedes usar el Pipe en las plantillas de Angular:

````html

<p>{{ texto | nombreDelPipe }}</p>

````

4. Registrar el Pipe: Asegurarse de que el Pipe esté declarado en el módulo correspondiente (por ejemplo, app.module.ts):

````typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { NombreDelPipe } from './nombre-del-pipe.pipe';

@NgModule({
  declarations: [
    AppComponent,
    NombreDelPipe
  ],
  imports: [BrowserModule],
  bootstrap: [AppComponent]
})
export class AppModule {}

````

## Ejemplo de un Pipe personalizado
 - Supongamos que quieres crear un Pipe que invierte una cadena de texto:

1. Implementación del Pipe:

````typescript

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'invertirTexto'
})
export class InvertirTextoPipe implements PipeTransform {
  transform(value: string): string {
    return value.split('').reverse().join('');
  }
}

````

2. Uso en el Template:

````html

<p>{{ 'Angular' | invertirTexto }}</p> <!-- Salida: "ralugnA" -->

````


## Angular Router

## Características Clave

1. Routing:

- Permite definir rutas en la aplicación, mapeando URLs a componentes específicos.
- Se configura en el módulo de enrutamiento.

2. outerModule:

- El módulo RouterModule es el núcleo del sistema de enrutamiento. Se debe importar en el módulo principal de la aplicación.
- Se utiliza para definir las rutas mediante RouterModule.forRoot(routes).

3. Rutas Hijas:

- Permiten crear jerarquías de rutas, donde una ruta puede tener subrutas, lo que ayuda a estructurar la aplicación de manera más modular.

4. Navegación:

- Proporciona métodos para navegar programáticamente entre rutas, como this.router.navigate(['ruta']).

5. Guards:

- Los guards permiten proteger rutas, impidiendo el acceso a ciertas partes de la aplicación según condiciones específicas (como autenticación).

6. Lazy Loading:

- Permite cargar módulos solo cuando son necesarios, mejorando el rendimiento al reducir el tamaño del bundle inicial.




## Ejemplo de Configuración de Angular Router

1. Instalar el Router: Asegurarse de que el proyecto tenga Angular Router:

````bash

ng add @angular/router

````
2. Definir Rutas: Crear un archivo de enrutamiento (por ejemplo, app-routing.module.ts):

````typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

````

3. Usar el Router Outlet: Añadir <router-outlet></router-outlet> en tu plantilla principal (por ejemplo, app.component.html) donde se cargarán los componentes de las rutas:

````html

<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/about">About</a>
</nav>
<router-outlet></router-outlet>
````

4. Navegación: Navegar entre las rutas usando los enlaces definidos:

````html

<a routerLink="/about">Ir a About</a>

````
