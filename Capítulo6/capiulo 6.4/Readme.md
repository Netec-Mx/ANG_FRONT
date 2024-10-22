## Práctica 11. Uso de ViewChild y Referencias Locales en Angular

## Objetivo

- Aprender a utilizar ViewChild y referencias locales en Angular para interactuar con elementos del DOM y componentes desde la lógica de un componente.

## Tarea 1.

1. Configurar el Proyecto

- Crea un nuevo proyecto Angular:

```bash

ng new taller-viewchild
```

```bash
cd taller-viewchild
ng serve
```

2. Crear un Componente para el Taller

- Genera un nuevo componente llamado contador

```bash

ng generate component contador
```

3. Implementar el Componente de Contador

- Abre contador.component.ts y añade la lógica para un contador simple que se pueda incrementar y restar:

```typescript
import { Component, ViewChild } from '@angular/core';

@Component({
  selector: 'app-contador',
  templateUrl: './contador.component.html',
})
export class ContadorComponent {
  @ViewChild('inputValue') inputElement!: HTMLInputElement;
  contador: number = 0;

  incrementar() {
    const incremento = Number(this.inputElement.value) || 0;
    this.contador += incremento;
  }

  decrementar() {
    const decremento = Number(this.inputElement.value) || 0;
    this.contador -= decremento;
  }
}
```

4. Crear la Plantilla del Componente de Contador

- Abre contador.component.html y construye la interfaz para el contador:

```html

<h2>Contador: {{ contador }}</h2>
<input #inputValue type="number" placeholder="Incrementar/Decrementar" />
<button (click)="incrementar()">Incrementar</button>
<button (click)="decrementar()">Decrementar</button>

```

5. Modificar el Componente Principal

- Abre app.component.html y añade el componente de contador:

```html

<h1>Ejemplo de ViewChild y Referencias Locales</h1>
<app-contador></app-contador>

```

6. Probar la Aplicación

- Asegúrate de que todo esté guardado y vuelve a cargar la aplicación en http://localhost:4200.

## Resultado Esperado
- Una aplicación que permite a los usuarios:
    - Ingresar un número en un input.
    - Incrementar y decrementar un contador en la pantalla al hacer clic en los botones.
    - Usar ViewChild para acceder al valor del input y referencias locales para manejar la lógica del contador.


