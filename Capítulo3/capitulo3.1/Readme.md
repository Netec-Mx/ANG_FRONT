# Práctica 3.1. Binding con Angular

## Objetivos de la práctica:
- Implementar diferentes tipos de binding en Angular.

## Duración aproximada:
- 30 minutos.

## Instrucciones

## Interpolación

1. Definir una propiedad en el componente: En el componente TypeScript (por ejemplo, app.component.ts), definir una propiedad:

````typescript

    import { Component } from '@angular/core';

    @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
    })
    export class AppComponent {
    title = 'Hola, Angular!';
    }

````
2. Usar interpolación en el template: En el archivo HTML correspondiente (por ejemplo, app.component.html), usar la interpolación para mostrar la propiedad: 

````html

<h1>{{ title }}</h1>

````

## Template Reference Variables

1. Definir una template reference variable.

```html

<input #nombreInput type="text">
<button (click)="saludar(nombreInput.value)">Saludar</button>
<p>{{ mensaje }}</p>

```

2. Componente TypeScript.

    - En el archivo del componente (por ejemplo, app.component.ts):

```typescript

    import { Component } from '@angular/core';

    @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
    })
    export class AppComponent {
    mensaje: string = '';

    saludar(nombre: string) {
        this.mensaje = `Hola, ${nombre}!`;
    }
    }

```

## Property Binding en Angular

1. Definir una propiedad en el componente: En tu componente TypeScript (por ejemplo, app.component.ts), define una propiedad:

```typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  imagenUrl: string = 'https://example.com/imagen.jpg';
}

```

2. Usar property binding en el template: En el archivo HTML correspondiente (por ejemplo, app.component.html), utilizar property binding para enlazar la propiedad:

``` html

<img [src]="imagenUrl" alt="Imagen de ejemplo">

```

## Event Binding en Angular

1. Definir un método en el componente: En el componente typeScript (por ejemplo, app.component.ts), define un método:

```typescript

    import { Component } from '@angular/core';

    @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
    })
    export class AppComponent {
    mensaje: string = '';

    saludar() {
        this.mensaje = '¡Hola, Angular!';
    }
    }

```

2. Usar Event binding en el template: En el archivo HTML correspondiente (por ejemplo, app.component.html), usar event binding:

```html

    <button (click)="saludar()">Saludar</button>
    <p>{{ mensaje }}</p>

```

## Two way binding con Angular

1. Importar FormsModule: Asegúrate de que FormsModule esté importado en el módulo. Abrir app.module.ts y añadir la importación:

```typescript

    import { FormsModule } from '@angular/forms';

    @NgModule({
    imports: [
        FormsModule,
        // otros módulos
    ],
    })
    export class AppModule { }

```

2. Definir una propiedad en el componente: En el componente typeScript (por ejemplo, app.component.ts), definir una propiedad:

```typescript

    import { Component } from '@angular/core';

    @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
    })
    export class AppComponent {
    nombre: string = '';
    }

```

3. Usar Two-Way binding en el template: En el archivo HTML correspondiente (por ejemplo, app.component.html), utilizar ngModel para enlazar la propiedad:

```html

    <input [(ngModel)]="nombre" placeholder="Escribe tu nombre">
    <p>Hola, {{ nombre }}!</p>

```

## Property binding entre componentes en Angular

1. Crear el componente Hijo: Primero, crear un componente que recibirá los datos. Por ejemplo, hijo.component.ts:

```typescript

import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-hijo',
  template: `<p>Mensaje del padre: {{ mensaje }}</p>`,
})
export class HijoComponent {
  @Input() mensaje: string = '';
}

```

2. Crear el componente Padre: Ahora, crear un componente Padre que pasará datos al componente Hijo. Por ejemplo, padre.component.ts:


```typescript

    import { Component } from '@angular/core';

    @Component({
    selector: 'app-padre',
    template: `
        <h1>Componente Padre</h1>
        <app-hijo [mensaje]="mensajeDelPadre"></app-hijo>
    `,
    })
    export class PadreComponent {
    mensajeDelPadre: string = 'Hola desde el componente padre!';
    }

```

3. Incluir el componente Hijo en el Padre: Asegurarse de que el componente Hijo esté declarado en el mismo módulo que el componente padre (usualmente app.module.ts):

``` typescript

    import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { PadreComponent } from './padre/padre.component';
import { HijoComponent } from './hijo/hijo.component';

@NgModule({
  declarations: [
    AppComponent,
    PadreComponent,
    HijoComponent,
  ],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule {}

```

4. Usar el componente Padre en la aplicación: Finalmente, utilizar el componente Padre en la aplicación, por ejemplo, en app.component.html:

```html

    <app-padre></app-padre>

```

## Event binding entre componentes en Angular

1. Crear el componente Hijo: Primero, definir un componente hijo que emitirá un evento. Por ejemplo, hijo.component.ts:

`````typescript
import { Component, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-hijo',
  template: `<button (click)="enviarMensaje()">Enviar Mensaje</button>`,
})
export class HijoComponent {
  @Output() mensajeEmitido = new EventEmitter<string>();

  enviarMensaje() {
    this.mensajeEmitido.emit('Hola desde el componente hijo!');
  }
}
`````

2. Crear el componente Padre: Ahora, crear un componente padre que escuchará el evento del componente hijo. Por ejemplo, padre.component.ts:

`````typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-padre',
  template: `
    <h1>Componente Padre</h1>
    <app-hijo (mensajeEmitido)="recibirMensaje($event)"></app-hijo>
    <p>Mensaje recibido: {{ mensajeDelHijo }}</p>
  `,
})
export class PadreComponent {
  mensajeDelHijo: string = '';

  recibirMensaje(mensaje: string) {
    this.mensajeDelHijo = mensaje;
  }
}

`````

3. Incluir el componente Hijo en el Padre: Asegurarse de que el componente Hijo esté declarado en el mismo módulo que el componente Padre (usualmente en app.module.ts):

`````typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { PadreComponent } from './padre/padre.component';
import { HijoComponent } from './hijo/hijo.component';

@NgModule({
  declarations: [
    AppComponent,
    PadreComponent,
    HijoComponent,
  ],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule {}

`````

4. Usar el componente Padre en la aplicación: Finalmente, utilizar el componente padre en la aplicación, por ejemplo, en app.component.html:

`````html

<app-padre></app-padre>

`````


