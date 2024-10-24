# Práctica 2. Creación de un componente manualmente

## Objetivo de la práctica:
 - Crear un componente en Angular de forma manual.
 - Aprender a definir la estructura y el comportamiento del componente.
 - Integrar el componente a una aplicación existente.

## Duración apróximada:
- 25 minutos.

## Instrucciones

## Creación del componente
1. Crear la carpeta del componente:

    - Navegar a la carpeta donde deseas crear el componente (por ejemplo, src/app).
    - Crear una nueva carpeta para tu componente, por ejemplo, mi-componente.

2. Crear el archivo TypeScript:

    - Dentro de la carpeta mi-componente, crear un archivo llamado mi-componente.component.ts.
    - Agregar el siguiente código:

    ````typescript
    import { Component } from '@angular/core';

    @Component({
    selector: 'app-mi-componente',
    templateUrl: './mi-componente.component.html',
    styleUrls: ['./mi-componente.component.css']
    })
    export class MiComponenteComponent {
    // Lógica del componente
    }
    ````
3. Crear el archivo de template:

    - Crear un archivo llamado mi-componente.component.html.
    - Agregar el contenido HTML que deseas mostrar en tu componente.

4. Crear el archivo de estilos:

    - Crear un archivo llamado mi-componente.component.css.
    - Añadir los estilos específicos para tu componente.

5. Declarar el componente en el módulo:

    - Abrir el archivo app.module.ts y añadir tu nuevo componente a la sección de declarations:
    
    ````typescript
    import { MiComponenteComponent } from './mi-componente/mi-componente.component';

    @NgModule({
    declarations: [
        MiComponenteComponent,
        // otros componentes
    ],
    // otras propiedades
    })
    export class AppModule { }
    ````

6. Usar el componente:

    - En el template de otro componente (por ejemplo, app.component.html), usar el selector de tu nuevo componente:

    ````html
    <app-mi-componente></app-mi-componente>
    ````


## Creación de un componente con Angular CLI

1. Abrir la Terminal:

    - Navegar a la carpeta raíz de tu proyecto Angular.

2. Ejecutar el comando de generación:

    - Usar el siguiente comando para crear un nuevo componente:

``
    ng generate component mi-componente
``

    - O simplemente

``
    ng g c mi-componente
``

3. Archivos generados:

    - Angular CLI generará automáticamente:

        - mi-componente.component.ts
        - mi-componente.component.html
        - mi-componente.component.css
        - mi-componente.component.spec.ts

    - Además, el nuevo componente se añadirá automáticamente a las declaraciones en el módulo correspondiente (usualmente app.module.ts).

4. Usar el componente:

    - Abrir el archivo app.component.html o el template donde desees utilizar el nuevo componente y añade:

````html
    <app-mi-componente></app-mi-componente>
````

## Instalación de Bootstrap con npm

1. Abrir la Terminal: Asegúrate de estar en la carpeta raíz de tu proyecto Angular.

2. Instalar Bootstrap: Ejecutar el siguiente comando en la terminal:

    - npm install bootstrap

## Incluir Bootstrap en el Proyecto

3. Modificar angular.json: Abrir el archivo angular.json y buscar la sección styles. Añadir la ruta de Bootstrap como sigue:

```json
    "styles": [
    "src/styles.css",
    "node_modules/bootstrap/dist/css/bootstrap.min.css"
    ],
```

## Usar Bootstrap en tu aplicación

4. Reiniciar el Servidor: Si el servidor de desarrollo está en ejecución, reinícialo para que los cambios surtan efecto.

5. Aplicar clases de Bootstrap: Ahora puedes utilizar las clases de Bootstrap en tus componentes. Por ejemplo, en app.component.html:

````html
    <button class="btn btn-primary">Botón Bootstrap</button>
````

## Opcional: Integrar Bootstrap JavaScript

Si necesitas componentes de JavaScript de Bootstrap (como modales o dropdowns), puedes instalar jQuery y Popper.js:

1. Instalar jQuery y Popper.js:

``
    npm install jquery popper.js
``

2. Modificar angular.json: Añade jQuery y Popper.js en la sección scripts:

```json
    "scripts": [
    "node_modules/jquery/dist/jquery.min.js",
    "node_modules/popper.js/dist/umd/popper.min.js",
    "node_modules/bootstrap/dist/js/bootstrap.min.js"
    ]
```
