# Práctica 10. Estructurando aplicaciones Angular: Uso de módulos para la modularidad


# Objetivo
- Crear una aplicación Angular que gestione una lista de tareas dividiendo la funcionalidad en módulos, específicamente utilizando un módulo de tareas y un módulo compartido.

  
1. Módulo Raíz

AppModule: Es el módulo principal de una aplicación Angular, definido en app.module.ts. Aquí se declaran todos los componentes, directivas y servicios que se usarán en la aplicación. Se importa el BrowserModule, que es necesario para ejecutar aplicaciones en el navegador.

2. Módulos de Funcionalidad

Feature Modules: Estos son módulos que agrupan funcionalidades específicas de la aplicación. Por ejemplo, puedes tener un módulo para la gestión de usuarios (UserModule) o un módulo para las tareas (TasksModule). Esto mejora la mantenibilidad y la escalabilidad de la aplicación.

3. Módulos Compartidos

Shared Module: Se utiliza para declarar y exportar componentes, directivas y pipes que se usarán en múltiples módulos de la aplicación. Esto evita la duplicación de código y facilita la reutilización.

4. Módulos de Terceros

Angular permite importar módulos de bibliotecas de terceros, como Angular Material o PrimeNG, que ofrecen componentes y servicios listos para usar. Estos se integran en la aplicación importando el módulo correspondiente.

5. Lazy Loading (Carga Perezosa)

Angular soporta la carga perezosa de módulos, lo que significa que un módulo solo se carga cuando es necesario, en lugar de cargarlo al inicio. Esto mejora el rendimiento de la aplicación, especialmente en aplicaciones grandes.

6. Declaraciones, Importaciones y Exportaciones

Declarations: Aquí se declaran los componentes, directivas y pipes que pertenecen al módulo.
Imports: Se utilizan para importar otros módulos que se requieren en el módulo actual.
Exports: Permiten que otros módulos accedan a los componentes, directivas y pipes declarados en el módulo.

7. Ejemplo de un Módulo

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

## Ejercicio Práctico Usando Módulos en Angular

## Pasos a seguir
1. Configurar el Proyecto:

- Crea un nuevo proyecto Angular:
```bash

ng new lista-tareas
```
```bash
cd lista-tareas
ng serve
```

2. Generar Módulo de Tareas:

- Crea un módulo para gestionar las tareas:
```bash

ng generate module tareas
```

3. Generar el Componente de Tareas:

- Dentro del módulo de tareas, genera un componente:
```bash

ng generate component tareas/tarea
```

4. Generar Módulo Compartido:

- Crea un módulo compartido para reutilizar componentes y directivas:
```bash

ng generate module shared
```

5. Agregar un Componente en el Módulo Compartido:

- Genera un componente que se usará en ambos módulos, como un componente de entrada para agregar tareas:
```bash

ng generate component shared/tarea-input
```

6. Implementar el Componente de Entrada de Tarea:

- Abre tarea-input.component.ts y añade la lógica:

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

7. Implementar el Componente de Tarea:

- Abre tarea.component.ts y agrega la lógica para gestionar la lista de tareas:
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
8. Configurar los Módulos:

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

- Shared Module (shared.module.ts): Asegúrate de que contenga el componente de entrada, que ya lo hemos añadido en el módulo de tareas.

9. Integrar el Módulo de Tareas en el Módulo Principal:

- Abre app.module.ts y añade el TareasModule:
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

10. Modificar el Componente Raíz:

- Abre app.component.html y agrega el componente de tareas:
```html

<app-tarea></app-tarea>
```

11. Probar la Aplicación:

- Asegúrate de que todo esté guardado y vuelve a cargar la aplicación en http://localhost:4200.
