# Práctica 4. Instalación de PrimeNG y PrimeIcons

## Objetivos de la práctica:
- Instalar y configurar PrimeNG y Primelcons en tu proyecto Angular.
- Aprender a integrar los conjuntos de estos componentes y estilos en tu aplicación.

## Duración aproximada:
- 20 minutos.

## Instrucciones:

1. Primero, tener PrimeNG y PrimeIcons instalados en el proyecto. Si aún no lo has hecho, ejecutar:

````bash

npm install primeng primeicons
````
2. Configuración de Angular

- Importar Módulos
- Abrir el archivo app.module.ts y añadir los módulos que deseas usar. Por ejemplo, si deseas usar un botón y un campo de entrada, el archivo podría verse así:

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

- Añadir los estilos de PrimeNG en el archivo styles.css o styles.scss. Puedes hacerlo así:

````css
@import "~primeng/resources/themes/saga-blue/theme.css"; /* Cambia "saga-blue" por el tema que prefieras */
@import "~primeng/resources/primeng.min.css";
@import "~primeicons/primeicons.css";
````

4. Usar Componentes en las Plantillas

- Ahora puedes empezar a usar los componentes de PrimeNG en los archivos de plantilla. Por ejemplo, en app.component.html:

````html
<div class="p-fluid">
  <h2>Ejemplo de PrimeNG</h2>
  <p-inputText placeholder="Ingrese su nombre" [(ngModel)]="nombre"></p-inputText>
  <p-button label="Enviar" icon="pi pi-check" (click)="enviar()"></p-button>
</div>
````
5. Lógica en el Componente

- Asegurarse de tener la lógica necesaria en el componente. Aquí hay un ejemplo básico en app.component.ts:

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

- Finalmente, ejecutar la aplicación con:

````bash
ng serve

````

- Visita http://localhost:4200 y deberías ver el formulario con los componentes de PrimeNG.

## Ejemplo de otros Componentes

- Puedes integrar otros componentes de PrimeNG siguiendo un patrón similar. Solo importa el módulo correspondiente en la app.module.ts y utilizar su etiqueta en las plantillas. 

- Aquí hay algunos ejemplos de componentes populares:

- Tabla: TableModule
- Diálogo: DialogModule
- Menú: MenuModule
- Gráfico: ChartModule
