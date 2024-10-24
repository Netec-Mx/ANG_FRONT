# Práctica 9. Implementación de Arquitectura Limpia en Angular

## Objetivo de la práctica

- Implementar la Arquitectura Limpia en Angular.
- Crear un sistema que permita añadir, listar y eliminar tareas.
- Mantener una separación clara entre las capas de entidades, casos de uso, interfaces e infraestructura.

## Duración aproximada
- 30 minutos.

## Instrucciones
## Estructura de Carpetas

- Una implementación típica de Arquitectura Limpia en Angular podría tener la siguiente estructura de carpetas:

```arduino

src/
├── app/
│   ├── core/               // Servicios, modelos y lógica de negocio
│   ├── features/           // Módulos de características específicas
│   ├── shared/             // Componentes y servicios reutilizables
│   ├── layout/             // Componentes de diseño y estructura
│   └── app.module.ts       // Módulo principal de la aplicación
└── assets/                 // Recursos estáticos (imágenes, etc.)

```

## Capas en la Arquitectura Limpia

1. Capa de Entidades:

Define los modelos y las reglas de negocio. Por ejemplo, podrías tener clases que representen las entidades de tu aplicación, como Usuario, Producto, etc.

```typescript

export class Usuario {
  constructor(public id: number, public nombre: string) {}
}
```

2. Capa de Casos de Uso:

Contiene la lógica específica de la aplicación, como operaciones de creación, lectura, actualización y eliminación (CRUD). Aquí es donde se implementan los casos de uso.

```typescript

import { Injectable } from '@angular/core';
import { Usuario } from '../models/usuario.model';

@Injectable({
  providedIn: 'root',
})
export class UsuarioService {
  private usuarios: Usuario[] = [];

  agregarUsuario(usuario: Usuario) {
    this.usuarios.push(usuario);
  }

  obtenerUsuarios(): Usuario[] {
    return this.usuarios;
  }
}
```

3. Capa de Interfaces:

Se encarga de manejar la interacción con el usuario y la comunicación con el servicio. Aquí se implementan componentes y módulos de Angular.

```typescript

import { Component } from '@angular/core';
import { UsuarioService } from '../core/usuario.service';
import { Usuario } from '../models/usuario.model';

@Component({
  selector: 'app-usuario-lista',
  templateUrl: './usuario-lista.component.html',
})
export class UsuarioListaComponent {
  usuarios: Usuario[];

  constructor(private usuarioService: UsuarioService) {
    this.usuarios = this.usuarioService.obtenerUsuarios();
  }
}
```

4. Capa de Infraestructura:

Maneja la persistencia de datos (por ejemplo, servicios HTTP para interactuar con APIs). Esta capa se conecta a las capas internas pero no debe afectar la lógica de negocio.

```typescript

import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class ApiService {
  constructor(private http: HttpClient) {}

  obtenerDatos() {
    return this.http.get('https://api.example.com/usuarios');
  }
}
```


## Ejercicio: Aplicación de Gestión de Tareas

## Estructura del Proyecto

1. Crear la estructura de carpetas:
```plaintext
Copy code
src/
├── app/
│   ├── core/
│   │   ├── models/
│   │   ├── services/
│   ├── features/
│   │   └── tasks/
│   ├── shared/
│   ├── layout/
│   └── app.module.ts

```

2. Paso 1: Definir la entidad.
- Crear el modelo de tarea en src/app/core/models/task.model.ts:

```typescript
Copy code
export class Task {
  constructor(public id: number, public title: string, public completed: boolean = false) {}
}

```

3. Paso 2: Crear el servicio (Capa de casos de uso).

- Crear un servicio para gestionar las tareas en src/app/core/services/task.service.ts:

```typescript
Copy code
import { Injectable } from '@angular/core';
import { Task } from '../models/task.model';

@Injectable({
  providedIn: 'root',
})
export class TaskService {
  private tasks: Task[] = [];
  private nextId: number = 1;

  addTask(title: string): Task {
    const newTask = new Task(this.nextId++, title);
    this.tasks.push(newTask);
    return newTask;
  }

  getTasks(): Task[] {
    return this.tasks;
  }

  deleteTask(id: number): void {
    this.tasks = this.tasks.filter(task => task.id !== id);
  }
}
```

4. Paso 3: Crear componentes (Capa de interfaces).

- Crear un componente para gestionar las tareas en src/app/features/tasks/task-list.component.ts:

```typescript
Copy code
import { Component, OnInit } from '@angular/core';
import { TaskService } from '../../core/services/task.service';
import { Task } from '../../core/models/task.model';

@Component({
  selector: 'app-task-list',
  template: `
    <h2>Task List</h2>
    <input [(ngModel)]="taskTitle" placeholder="New task" />
    <button (click)="addTask()">Add Task</button>
    <ul>
      <li *ngFor="let task of tasks">
        {{ task.title }}
        <button (click)="deleteTask(task.id)">Delete</button>
      </li>
    </ul>
  `,
})
export class TaskListComponent implements OnInit {
  tasks: Task[] = [];
  taskTitle: string = '';

  constructor(private taskService: TaskService) {}

  ngOnInit(): void {
    this.loadTasks();
  }

  loadTasks(): void {
    this.tasks = this.taskService.getTasks();
  }

  addTask(): void {
    if (this.taskTitle) {
      this.taskService.addTask(this.taskTitle);
      this.taskTitle = '';
      this.loadTasks();
    }
  }

  deleteTask(id: number): void {
    this.taskService.deleteTask(id);
    this.loadTasks();
  }
}
```

5. Paso 4: Integrar el componente en la aplicación.
- Agregar el componente al módulo principal en src/app/app.module.ts:

```typescript
Copy code
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { TaskListComponent } from './features/tasks/task-list.component';
import { TaskService } from './core/services/task.service';

@NgModule({
  declarations: [
    AppComponent,
    TaskListComponent,
  ],
  imports: [
    BrowserModule,
    FormsModule,
  ],
  providers: [TaskService],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

6. Paso 5: Probar la aplicación.

- Ejecutar la aplicación con ng serve.
- Acceder a http://localhost:4200 y probar añadir, listar y eliminar tareas.
