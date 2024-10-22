## Práctica 9. Usando Directivas Sin Personalizadas

## Objetivo

- Crear una pequeña aplicación Angular que muestre una lista de tareas con funcionalidad para agregar, marcar como completadas y eliminar tareas. Utilizaremos las directivas   

- *ngFor, *ngIf, *ngClass, y *ngModel.

Tarea. 1. Configuración del Proyecto

 - Crea un nuevo proyecto Angular:

```bash
ng new tareas-app
cd tareas-app
ng serve
```

 - Abrir Tel proyecto en tu editor de código.

## Tarea 2. Crear el Componente

- Genera un nuevo componente para las tareas:

```bash
ng generate component tareas

```

## Tarea 3. Implementar la Lógica en el Componente
- Abrir tareas.component.ts y agregar la siguiente lógica:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-tareas',
  templateUrl: './tareas.component.html',
  styleUrls: ['./tareas.component.css']
})
export class TareasComponent {
  nuevaTarea: string = '';
  tareas: { nombre: string, completada: boolean }[] = [];

  AgregarTarea() {
    if (this.nuevaTarea.trim()) {
      this.tareas.push({ nombre: this.nuevaTarea, completada: false });
      this.nuevaTarea = '';
    }
  }

  eliminarTarea(index: number) {
    this.tareas.splice(index, 1);
  }
}

```

## Tarea 4. Crear la Plantilla HTML

- Abrir tareas.component.html y agregar el siguiente código:

```html
<h2>Lista de Tareas</h2>

<input [(ngModel)]="nuevaTarea" placeholder="Agregar nueva tarea" />
<button (click)="agregarTarea()">Agregar</button>

<ul>
  <li *ngFor="let tarea of tareas; let i = index">
    <span [ngClass]="{ 'completada': tarea.completada }" (click)="tarea.completada = !tarea.completada">
      {{ tarea.nombre }}
    </span>
    <button (click)="eliminarTarea(i)">Eliminar</button>
  </li>
</ul>

<p *ngIf="tareas.length === 0">No hay tareas en la lista.</p>
```

## Tarea5. Estilos CSS (Opcional)

- Abre tareas.component.css y agrega algo de estilo:

```css
.completada {
  text-decoration: line-through;
  color: gray;
}
```

## Tarea 6. Integrar el Componente en la Aplicación

- Asegúrate de que tu componente tareas se incluya en el app.component.html:

```html
Copy code
<app-tareas></app-tareas>
```

## Tarea 7. Probar la Aplicación

- Ejecuta la aplicación si no lo has hecho:

```bash
Copy code
ng serve

```

## Ejercicio Independiente: Crear Directivas Personalizadas

- Ejercicio: Directiva de Resaltado de Texto

1. Genera la Directiva:

- Usa Angular CLI para generar una directiva llamada resaltado.

```bash
Copy code
ng generate directive resaltado
```

2. Implementa la Lógica:

- Abre resaltado.directive.ts y agrega la siguiente lógica para cambiar el color del texto al pasar el ratón:

```typescript
Copy code
import { Directive, ElementRef, Renderer2, HostListener } from '@angular/core';

@Directive({
  selector: '[appResaltado]'
})
export class ResaltadoDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', 'yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.renderer.removeStyle(this.el.nativeElement, 'backgroundColor');
  }
}

```

3. Usa la Directiva:

- En cualquier componente de tu aplicación, aplica la directiva:

```html
Copy code
<p appResaltado>Este texto se resaltará al pasar el ratón.</p>
```

4. Probar la Directiva:

- Ejecuta la aplicación y verifica que el texto se resalte al pasar el ratón.
