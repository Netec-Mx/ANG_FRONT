# Práctica 5. Mejorando la funcionalidad y presentación: Uso de directivas en Angular

## Objetivo de la práctica:

- Crear una pequeña aplicación Angular que muestre una lista de tareas con funcionalidad para agregar, marcar como completadas y eliminar tareas. 

## Duración aproximada
- 30 minutos.

## Instrucciones

- *ngFor, *ngIf, *ngClass, y *ngModel.

1. Configuración del proyecto.

 - Crear un nuevo proyecto Angular:

```bash
ng new tareas-app
cd tareas-app
ng serve
```

 - Abrir Tel proyecto en el editor de código.

2. Crear el Componente.

- Generar un nuevo componente para las tareas:

```bash
ng generate component tareas

```

3. Implementar la lógica en el componente.
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

4. Crear la plantilla HTML.

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

5. Estilos CSS (Opcional).

- Abrir tareas.component.css y agregar algo de estilo:

```css
.completada {
  text-decoration: line-through;
  color: gray;
}
```

6. Integrar el componente en la aplicación.

- Asegúrate de que el componente tareas se incluya en el app.component.html:

```html
Copy code
<app-tareas></app-tareas>
```

7. Probar la aplicación.

- Ejecutar la aplicación si no lo has hecho:

```bash
Copy code
ng serve

```

## Ejercicio independiente: Crear directivas personalizadas

- Ejercicio: Directiva de resaltado de texto

1. Generar la directiva:

- Usar Angular CLI para generar una directiva llamada resaltado.

```bash
Copy code
ng generate directive resaltado
```

2. Implementar la lógica:

- Abrir resaltado.directive.ts y agregar la siguiente lógica para cambiar el color del texto al pasar el ratón:

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

3. Usar la directiva:

- En cualquier componente de tu aplicación, aplica la directiva:

```html
Copy code
<p appResaltado>Este texto se resaltará al pasar el ratón.</p>
```

4. Probar la directiva:

- Ejecutar la aplicación y verificar que el texto se resalte al pasar el ratón.
