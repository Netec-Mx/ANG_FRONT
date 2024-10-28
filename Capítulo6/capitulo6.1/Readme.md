# Práctica 6.1 Estructurando aplicaciones Angular: Uso de módulos para la modularidad

## Objetivo de la práctica:
- Crear una aplicación Angular que gestione una lista de tareas dividiendo la funcionalidad en módulos, específicamente utilizando un módulo de tareas y un módulo compartido.

## Duración  aproximada:
- 30 minutos.

## Instrucciones:

1. Módulo Raíz.

AppModule: Es el módulo principal de una aplicación Angular, definido en app.module.ts. Aquí se declaran todos los componentes, directivas y servicios que se usarán en la aplicación. Se importa el BrowserModule, que es necesario para ejecutar aplicaciones en el navegador.

2. Módulos de funcionalidad.

Feature Modules: Estos son módulos que agrupan funcionalidades específicas de la aplicación. Por ejemplo, puedes tener un módulo para la gestión de usuarios (UserModule) o un módulo para las tareas (TasksModule). Esto mejora la mantenibilidad y la escalabilidad de la aplicación.

3. Módulos compartidos.

Shared Module: Se utiliza para declarar y exportar componentes, directivas y pipes que se usarán en múltiples módulos de la aplicación. Esto evita la duplicación de código y facilita la reutilización.

4. Módulos de terceros.

Angular permite importar módulos de bibliotecas de terceros, como Angular Material o PrimeNG, que ofrecen componentes y servicios listos para usar. Estos se integran en la aplicación importando el módulo correspondiente.

5. Lazy Loading (Carga Perezosa).

Angular soporta la carga perezosa de módulos, lo que significa que un módulo solo se carga cuando es necesario, en lugar de cargarlo al inicio. Esto mejora el rendimiento de la aplicación, especialmente en aplicaciones grandes.

6. Declaraciones, importaciones y exportaciones.

Declarations: Aquí se declaran los componentes, directivas y pipes que pertenecen al módulo.
Imports: Se utilizan para importar otros módulos que se requieren en el módulo actual.
Exports: Permiten que otros módulos accedan a los componentes, directivas y pipes declarados en el módulo.

7. Ejemplo de un módulo.

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { FeatureComponent } from './feature/feature.component';

@NgModule({
  declarations: [
    AppComponent,
    FeatureComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Ejercicio práctico usando módulos en Angular

## Pasos a seguir
1. Configurar el proyecto:

- Crear un nuevo proyecto Angular:
```bash

ng new lista-tareas
```
```bash
cd lista-tareas
ng serve
```

2. Generar módulo de tareas:

- Crear un módulo para gestionar las tareas:
```bash

ng generate module tareas
```

3. Generar el componente de tareas:

- Dentro del módulo de tareas, generar un componente:
```bash

ng generate component tareas/tarea
```

4. Generar módulo compartido:

- Crear un módulo compartido para reutilizar componentes y directivas:
```bash

ng generate module shared
```

5. Agregar un componente en el módulo compartido:

- Generar un componente que se usará en ambos módulos, como un componente de entrada para agregar tareas:
```bash

ng generate component shared/tarea-input
```

6. Implementar el componente de entrada de tarea:

- Abrir tarea-input.component.ts y añadir la lógica:

```typescript

import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-tarea-input',
  template: `
    <input [(ngModel)]="nuevaTarea" placeholder="Agregar nueva tarea" />
    <button (click)="agregarTarea()">Agregar</button>
  `
})
export class TareaInputComponent {
  nuevaTarea: string = '';
  @Output() tareaAgregada = new EventEmitter<string>();

  agregarTarea() {
    if (this.nuevaTarea.trim()) {
      this.tareaAgregada.emit(this.nuevaTarea);
      this.nuevaTarea = '';
    }
  }
}

```

7. Implementar el componente de tarea:

- Abrir tarea.component.ts y agregar la lógica para gestionar la lista de tareas:
```typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-tarea',
  template: `
    <app-tarea-input (tareaAgregada)="agregarTarea($event)"></app-tarea-input>
    <ul>
      <li *ngFor="let tarea of tareas">
        {{ tarea }}
        <button (click)="eliminarTarea(tarea)">Eliminar</button>
      </li>
    </ul>
    <p *ngIf="tareas.length === 0">No hay tareas en la lista.</p>
  `
})
export class TareaComponent {
  tareas: string[] = [];

  agregarTarea(tarea: string) {
    this.tareas.push(tarea);
  }

  eliminarTarea(tarea: string) {
    this.tareas = this.tareas.filter(t => t !== tarea);
  }
}
```
8. Configurar los módulos:

- Tareas Module (tareas.module.ts):
```typescript

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TareaComponent } from './tarea.component';
import { TareaInputComponent } from '../shared/tarea-input/tarea-input.component';
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    TareaComponent,
    TareaInputComponent
  ],
  imports: [
    CommonModule,
    FormsModule
  ],
  exports: [TareaComponent]
})
export class TareasModule { }

```

- Shared Module (shared.module.ts): Asegurarse de que contenga el componente de entrada, que ya se ha añadido en el módulo de tareas.

9. Integrar el módulo de tareas en el módulo principal:

- Abrir app.module.ts y añadir el TareasModule:
```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { TareasModule } from './tareas/tareas.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    TareasModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

10. Modificar el componente raíz:

- Abrir app.component.html y agregar el componente de tareas:
```html

<app-tarea></app-tarea>
```

11. Probar la aplicación:

- Comprobrar que todo esté guardado y volver a cargar la aplicación en http://localhost:4200.
