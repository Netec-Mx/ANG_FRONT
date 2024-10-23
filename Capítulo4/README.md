# Práctica 8. Instalación de PrimeNG y PrimeIcons

## Objetivos de la práctica:
- Instalar y configurar PrimeNG y Primelcons en tu proyecto Angular.
- Aprender a integrar los conjuntos de estos componentes y estilos en tu aplicación.

## Duración aproximada:


## Instrucciones:

1. Primero, asegúrate de tener PrimeNG y PrimeIcons instalados en tu proyecto. Si aún no lo has hecho, ejecutar:

````bash

npm install primeng primeicons
````
2. Configuración de Angular

- Importar Módulos
- Abrir tu archivo app.module.ts y añadir los módulos que deseas usar. Por ejemplo, si deseas usar un botón y un campo de entrada, tu archivo podría verse así:

````typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { ButtonModule } from 'primeng/button';
import { InputTextModule } from 'primeng/inputtext';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    ButtonModule,
    InputTextModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}

````

3. Agregar Estilos

- Asegúrate de añadir los estilos de PrimeNG en tu archivo styles.css o styles.scss. Puedes hacerlo así:

````css
@import "~primeng/resources/themes/saga-blue/theme.css"; /* Cambia "saga-blue" por el tema que prefieras */
@import "~primeng/resources/primeng.min.css";
@import "~primeicons/primeicons.css";
````

4. Usar Componentes en las Plantillas

- Ahora puedes empezar a usar los Componentes de PrimeNG en tus archivos de plantilla. Por ejemplo, en app.component.html:

````html
<div class="p-fluid">
  <h2>Ejemplo de PrimeNG</h2>
  <p-inputText placeholder="Ingrese su nombre" [(ngModel)]="nombre"></p-inputText>
  <p-button label="Enviar" icon="pi pi-check" (click)="enviar()"></p-button>
</div>
````
5. Lógica en el Componente

- Asegúrate de tener la lógica necesaria en tu componente. Aquí hay un ejemplo básico en app.component.ts:

````typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
})
export class AppComponent {
  nombre: string = '';

  enviar() {
    console.log('Nombre ingresado:', this.nombre);
  }
}
````

6. Ejecución de la aplicación

- Finalmente, ejecutar tu aplicación con:

````bash
ng serve

````

- Visita http://localhost:4200 y deberías ver tu formulario con los componentes de PrimeNG.

## Ejemplo de otros Componentes

- Puedes integrar otros componentes de PrimeNG siguiendo un patrón similar. Solo importa el módulo correspondiente en tu app.module.ts y utilizar su etiqueta en tus plantillas. 

- Aquí hay algunos ejemplos de componentes populares:

- Tabla: TableModule
- Diálogo: DialogModule
- Menú: MenuModule
- Gráfico: ChartModule
