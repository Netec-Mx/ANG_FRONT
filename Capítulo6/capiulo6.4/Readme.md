# Práctica 6.2. Uso de ViewChild y referencias locales en Angular

## Objetivo de la práctica

- Aprender a utilizar ViewChild y referencias locales en Angular para interactuar con elementos del DOM y componentes desde la lógica de un componente.

## Duración aproximada:
- 30 minutos

## Instrucciones

1. Configurar el proyecto.

- Crear un nuevo proyecto Angular:

```bash

ng new taller-viewchild
```

```bash
cd taller-viewchild
ng serve
```

2. Crear un componente para el taller.

- Generar un nuevo componente llamado contador.

```bash

ng generate component contador
```

3. Implementar el componente de contador.

- Abrir contador.component.ts y añadir la lógica para un contador simple que se pueda incrementar y restar:

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

4. Crear la plantilla del componente de contador.

- Abrir contador.component.html y construir la interfaz para el contador:

```html

<h2>Contador: {{ contador }}</h2>
<input #inputValue type="number" placeholder="Incrementar/Decrementar" />
<button (click)="incrementar()">Incrementar</button>
<button (click)="decrementar()">Decrementar</button>

```

5. Modificar el componente principal.

- Abrir app.component.html y añadir el componente de contador:

```html

<h1>Ejemplo de ViewChild y Referencias Locales</h1>
<app-contador></app-contador>

```

6. Probar la aplicación

- Asegurarse de que todo esté guardado y volver a cargar la aplicación en http://localhost:4200.

## Resultado Esperado
- Una aplicación que permite a los usuarios:
    - Ingresar un número en un input.
    - Incrementar y decrementar un contador en la pantalla al hacer clic en los botones.
    - Usar ViewChild para acceder al valor del input y referencias locales para manejar la lógica del contador.


