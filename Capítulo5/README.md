## Ejercicio Práctico Usando Directivas Sin Personalizadas

## Objetivo
1. Crear una pequeña aplicación Angular que muestre una lista de tareas con funcionalidad para agregar, marcar como completadas y eliminar tareas. Utilizaremos las directivas   

- *ngFor, *ngIf, *ngClass, y *ngModel.

1. Paso 1: Configuración del Proyecto

 - Crea un nuevo proyecto Angular:

```bash
ng new tareas-app
cd tareas-app
ng serve
```

 - Abre el proyecto en tu editor de código.

2. Paso 2: Crear el Componente

- Genera un nuevo componente para las tareas:

```bash
ng generate component tareas

```

3. Paso 3: Implementar la Lógica en el Componente
- Abre tareas.component.ts y agrega la siguiente lógica:

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

  agregarTarea() {
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

4. Paso 4: Crear la Plantilla HTML

- Abre tareas.component.html y agrega el siguiente código:

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

5. Paso 5: Estilos CSS (Opcional)

- Abre tareas.component.css y agrega algo de estilo:

```css
.completada {
  text-decoration: line-through;
  color: gray;
}
```

6. Paso 6: Integrar el Componente en la Aplicación

- Asegúrate de que tu componente tareas se incluya en el app.component.html:

```html
Copy code
<app-tareas></app-tareas>
```

7. Paso 7: Probar la Aplicación

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