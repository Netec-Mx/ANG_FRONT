## Práctica 6. Binding con Angular

## Objetivos
- Implementar diferentes tipos de binding en Angular.

## Interpolación

1. Definir una Propiedad en el Componente: En tu componente TypeScript (por ejemplo, app.component.ts), define una propiedad:

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
2. Usar Interpolación en el Template: En el archivo HTML correspondiente (por ejemplo, app.component.html), usar la interpolación para mostrar la propiedad: 

````html

<h1>{{ title }}</h1>

````

## Template Reference Variables

1. Definir una Template Reference Variable

```html

<input #nombreInput type="text">
<button (click)="saludar(nombreInput.value)">Saludar</button>
<p>{{ mensaje }}</p>

```

2. Componente TypeScript

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

1. Definir una Propiedad en el Componente: En tu componente TypeScript (por ejemplo, app.component.ts), define una propiedad:

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

2. Usar Property Binding en el Template: En el archivo HTML correspondiente (por ejemplo, app.component.html), utiliza property binding para enlazar la propiedad:

``` html

<img [src]="imagenUrl" alt="Imagen de ejemplo">

```

## Event Binding en Angular

1. Definir un Método en el Componente: En tu componente TypeScript (por ejemplo, app.component.ts), define un método:

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

2. Usar Event Binding en el Template: En el archivo HTML correspondiente (por ejemplo, app.component.html), usa event binding:

```html

    <button (click)="saludar()">Saludar</button>
    <p>{{ mensaje }}</p>

```

## Two way binding con Angular.

1. Importar FormsModule: Asegúrate de que FormsModule esté importado en tu módulo. Abrir app.module.ts y añadir la importación:

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

2. Definir una Propiedad en el Componente: En tu componente TypeScript (por ejemplo, app.component.ts), definir una propiedad:

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

3. Usar Two-Way Binding en el Template: En el archivo HTML correspondiente (por ejemplo, app.component.html), utilizar ngModel para enlazar la propiedad:

```html

    <input [(ngModel)]="nombre" placeholder="Escribe tu nombre">
    <p>Hola, {{ nombre }}!</p>

```

## Property Binding entre Componentes en Angular

1. Crear el Componente Hijo: Primero, crear un componente que recibirá los datos. Por ejemplo, hijo.component.ts:

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

2. Crear el Componente Padre: Ahora, crear un componente padre que pasará datos al componente hijo. Por ejemplo, padre.component.ts:


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

3. Incluir el Componente Hijo en el Padre: Asegúrate de que el componente hijo esté declarado en el mismo módulo que el componente padre (usualmente app.module.ts):

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

4. Usar el Componente Padre en la Aplicación: Finalmente, utilizar el componente padre en tu aplicación, por ejemplo, en app.component.html:

```html

    <app-padre></app-padre>

```

## Event Biding entre Componentes en Angular.

1. Crear el Componente Hijo: Primero, definir un componente hijo que emitirá un evento. Por ejemplo, hijo.component.ts:

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

2. Crear el Componente Padre: Ahora, crear un componente padre que escuchará el evento del componente hijo. Por ejemplo, padre.component.ts:

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

3. Incluir el Componente Hijo en el Padre: Asegúrate de que el componente hijo esté declarado en el mismo módulo que el componente padre (usualmente en app.module.ts):

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

4. Usar el Componente Padre en la Aplicación: Finalmente, utilizar el componente padre en tu aplicación, por ejemplo, en app.component.html:

`````html

<app-padre></app-padre>

`````


