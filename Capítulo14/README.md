# Práctica 14. Gestión de formularios en Angular: De plantillas a reactivos

## Objetivo de la práctica

- Implementar y gestionar formularios en Angular utilizando tanto enfoques basados en plantillas como reactivos.
- Aprender a crear y validar formularios, aplicando validaciones a los campos de entrada y gestionando el estado del formulario de manera efectiva.
- Realizar un ejercicio práctico en el que crearás un formulario de registro completo, aplicando los conceptos aprendidos y asegurando que tu aplicación maneje la entrada de datos de forma robusta y segura.

## Duración aproximada
- 40 minutos.

## Instrucciones

1. ¿Qué son los Formularios Basados en Plantillas?

- Los formularios basados en plantillas se construyen utilizando la estructura de plantillas de Angular y aprovechan el sistema de detección de cambios de Angular para gestionar los estados del formulario. En este enfoque, se utiliza la directiva ngModel para vincular los elementos del formulario a propiedades en el componente, permitiendo la comunicación bidireccional entre la vista y el modelo.

- *Ejemplo de un Formulario Básico*

```html

<form #miFormulario="ngForm" (ngSubmit)="onSubmit(miFormulario)">
  <label for="nombre">Nombre:</label>
  <input type="text" id="nombre" name="nombre" ngModel required>
  
  <button type="submit" [disabled]="miFormulario.invalid">Enviar</button>
</form>

```

En este ejemplo:

- #miFormulario="ngForm" crea una referencia al formulario que permite acceder a su estado.

- ngModel se utiliza para enlazar el campo de entrada nombre a la propiedad del modelo.

- El botón de envío está deshabilitado hasta que el formulario sea válido.


2. Ventajas de los Formularios Basados en Plantillas

- *Simplicidad*: La sintaxis es más directa y fácil de entender para formularios sencillos.

- *Menos Código*: Requiere menos configuración en comparación con los formularios reactivos, ya que gran parte de la lógica se maneja en la plantilla.

- *Ideal para Formulario Simples*: Perfecto para formularios que no requieren validaciones complejas o un manejo avanzado de estados.


3. Validaciones en Formularios Basados en Plantillas

- Angular ofrece varias directivas para realizar validaciones de forma sencilla. Las validaciones pueden ser de varios tipos, incluyendo requeridas, patrones o longitudes.

- *Ejemplo de Validaciones*

```html

<input type="text" id="email" name="email" ngModel required email>
<div *ngIf="miFormulario.controls.email?.invalid && (miFormulario.controls.email?.touched || miFormulario.controls.email?.dirty)">
  <small *ngIf="miFormulario.controls.email?.errors?.required">El email es requerido.</small>
  <small *ngIf="miFormulario.controls.email?.errors?.email">El formato del email es inválido.</small>
</div>
```

4. Manejo de Eventos

- El manejo de eventos en formularios basados en plantillas es sencillo. Se puede utilizar la directiva (ngSubmit) para manejar el evento de envío del formulario.

- *Ejemplo de Manejo de Eventos*

```typescript

onSubmit(form: NgForm) {
  console.log('Formulario enviado:', form.value);
}
```

5. Acceso a los Valores del Formulario

- Los valores del formulario se pueden acceder a través de la propiedad value del objeto del formulario. Esto permite capturar toda la información ingresada por el usuario.

6. Buenas Prácticas

- *Usar Validaciones*: Siempre que sea posible, agrega validaciones para mejorar la experiencia del usuario y garantizar la calidad de los datos.

- *Manejar el Estado del Formulario*: Utiliza las propiedades de estado (touched, dirty, valid, invalid) para mejorar la interacción del usuario.

- *Evitar Lógica Compleja en la Plantilla*: Mantén la lógica en el componente siempre que sea posible para que la plantilla sea más limpia y fácil de entender.


## Formularios Reactivos en Angular

1. ¿Qué son los Formularios Reactivos?

- Los formularios reactivos se construyen utilizando el módulo ReactiveFormsModule, que permite crear formularios utilizando una API programática. En lugar de vincular la plantilla directamente a los modelos a través de ngModel, se utiliza la clase FormGroup para representar el estado del formulario y FormControl para cada campo de entrada.

- *Ejemplo de un Formulario Reactivo*

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-mi-formulario',
  template: `
    <form [formGroup]="miFormulario" (ngSubmit)="onSubmit()">
      <label for="nombre">Nombre:</label>
      <input id="nombre" formControlName="nombre">
      <div *ngIf="miFormulario.get('nombre').invalid && miFormulario.get('nombre').touched">
        <small *ngIf="miFormulario.get('nombre').errors.required">El nombre es requerido.</small>
      </div>

      <button type="submit" [disabled]="miFormulario.invalid">Enviar</button>
    </form>
  `
})
export class MiFormularioComponent {
  miFormulario: FormGroup;

  constructor(private fb: FormBuilder) {
    this.miFormulario = this.fb.group({
      nombre: ['', Validators.required]
    });
  }

  onSubmit() {
    console.log('Formulario enviado:', this.miFormulario.value);
  }
}
```

2. Ventajas de los Formularios Reactivos

- *Control Total*: Proporciona un mayor control sobre el estado del formulario y la validación, permitiendo una lógica más compleja.

- *Escalabilidad*: Facilita la creación de formularios complejos y dinámicos, ideales para aplicaciones grandes.

- *Pruebas Sencillas*: La estructura basada en clases permite realizar pruebas unitarias más efectivas y fáciles de entender.

- *Cambio de Datos*: Soporta la programación reactiva, lo que significa que los cambios en los datos pueden ser escuchados y gestionados fácilmente.

3. Estructura de un Formulario Reactivo

- Un formulario reactivo está compuesto por varios elementos clave:

- *FormGroup*: Representa un grupo de controles. Se puede anidar para crear estructuras de formulario más complejas.

- *FormControl*: Representa un único control de entrada. Puede tener validaciones y un estado asociado (touched, dirty, valid).

- *FormArray*: Representa un conjunto de controles en un array, útil para formularios que requieren listas dinámicas.

4. Validaciones en Formularios Reactivos

- Las validaciones se pueden definir directamente en los controles o como un conjunto de validaciones en el grupo. Angular proporciona varios validadores integrados, y también se pueden crear validadores personalizados.

- *Ejemplo de Validaciones*

```typescript

this.miFormulario = this.fb.group({
  nombre: ['', [Validators.required, Validators.minLength(3)]],
  email: ['', [Validators.required, Validators.email]]
});

```

5. Manejo de Eventos

- El manejo de eventos en formularios reactivos se realiza a través de métodos en el componente. Por ejemplo, se pueden usar observables para reaccionar a los cambios en los valores del formulario.

- *Ejemplo de Manejo de Eventos*

```typescript

this.miFormulario.get('nombre').valueChanges.subscribe(value => {
  console.log('Nombre cambiado:', value);
});

```
6. Buenas Prácticas

- *Usar FormBuilder*: Facilita la creación de grupos y controles de formulario, haciendo el código más limpio.
- *Separar la Lógica*: Mantén la lógica del formulario en el componente para que la plantilla permanezca sencilla y enfocada en la presentación.
- *Pruebas Unitarias*: Aprovecha la estructura de los formularios reactivos para implementar pruebas unitarias efectivas.


## Validaciones de Form Input en Angular

1. Tipos de Validaciones

-  Angular ofrece varios tipos de validaciones, que se pueden clasificar en:

- *Validaciones Requeridas*: Aseguran que un campo no esté vacío.

- *Validaciones de Formato*: Verifican que el valor ingresado cumpla con un formato específico (como correos electrónicos o números).

- *Validaciones de Longitud*: Comprobar que la longitud del texto esté dentro de un rango especificado.

- *Validaciones Personalizadas*: Permiten crear reglas de validación a medida según las necesidades de la aplicación.

2. Validaciones en Formularios Basados en Plantillas

- En formularios basados en plantillas, las validaciones se configuran directamente en el HTML utilizando directivas. Por ejemplo:

```html

<form #miFormulario="ngForm" (ngSubmit)="onSubmit(miFormulario)">
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" ngModel required email>
  <div *ngIf="miFormulario.controls.email?.invalid && (miFormulario.controls.email?.touched || miFormulario.controls.email?.dirty)">
    <small *ngIf="miFormulario.controls.email?.errors?.required">El email es requerido.</small>
    <small *ngIf="miFormulario.controls.email?.errors?.email">El formato del email es inválido.</small>
  </div>

  <button type="submit" [disabled]="miFormulario.invalid">Enviar</button>
</form>
```

- En este ejemplo, el campo de email tiene dos validaciones: una requerida y una de formato.

3. Validaciones en Formularios Reactivos

- En formularios reactivos, las validaciones se definen en el componente usando la clase FormBuilder para crear un FormGroup con validadores asociados a cada FormControl.

- *Ejemplo de Validaciones en Formularios Reactivos*

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-mi-formulario',
  template: `
    <form [formGroup]="miFormulario" (ngSubmit)="onSubmit()">
      <label for="nombre">Nombre:</label>
      <input id="nombre" formControlName="nombre">
      <div *ngIf="miFormulario.get('nombre').invalid && miFormulario.get('nombre').touched">
        <small *ngIf="miFormulario.get('nombre').errors.required">El nombre es requerido.</small>
        <small *ngIf="miFormulario.get('nombre').errors.minlength">El nombre debe tener al menos 3 caracteres.</small>
      </div>

      <button type="submit" [disabled]="miFormulario.invalid">Enviar</button>
    </form>
  `
})
export class MiFormularioComponent {
  miFormulario: FormGroup;

  constructor(private fb: FormBuilder) {
    this.miFormulario = this.fb.group({
      nombre: ['', [Validators.required, Validators.minLength(3)]]
    });
  }

  onSubmit() {
    console.log('Formulario enviado:', this.miFormulario.value);
  }
}
```

4. Validaciones Personalizadas

- Angular permite la creación de validaciones personalizadas, lo que brinda mayor flexibilidad. Estas validaciones se implementan como funciones que devuelven un objeto de error si la validación falla.

- *Ejemplo de Validación Personalizada*

```typescript

import { AbstractControl, ValidationErrors } from '@angular/forms';

export function nombreValidator(control: AbstractControl): ValidationErrors | null {
  const valid = /^[A-Za-z]+$/.test(control.value);
  return valid ? null : { invalidNombre: true };
}
```

- Esta función puede ser utilizada en el formulario:

```typescript

this.miFormulario = this.fb.group({
  nombre: ['', [Validators.required, nombreValidator]]
});

```

5. Manejo de Errores y Mensajes

- Es importante proporcionar retroalimentación al usuario en caso de errores. Angular permite mostrar mensajes de error de manera condicional, basándose en el estado de validación de cada control.

- *Ejemplo de Mensajes de Error*

```html

<div *ngIf="miFormulario.get('nombre').invalid && miFormulario.get('nombre').touched">
  <small *ngIf="miFormulario.get('nombre').errors.required">El nombre es requerido.</small>
  <small *ngIf="miFormulario.get('nombre').errors.invalidNombre">El nombre solo puede contener letras.</small>
</div>
```

6. Buenas Prácticas

- *Validaciones Visuales*: Proporcionar mensajes claros y visibles para ayudar a los usuarios a corregir errores.

- *Uso de Validaciones*: Utilizar tanto validaciones integradas como personalizadas para cubrir diferentes casos de uso.

- *Manejo de Estados*: Usar estados como touched y dirty para mostrar errores solo después de que el usuario ha interactuado con el campo.


## Ejercicio Práctico: Creación de un Formulario de Registro en Angular

- *Objetivo*

- Desarrollar un formulario de registro que incluya campos para nombre, email y contraseña, utilizando tanto formularios basados en plantillas como formularios reactivos.

1. Paso 1: Crear el Proyecto Angular

- Instalar Angular CLI (si aún no lo tienes instalado):

```bash

npm install -g @angular/cli
```

- Crear un nuevo proyecto Angular:

```bash

ng new formulario-registro
```

- Entrar en el directorio del proyecto:

```bash

cd formulario-registro
```

- Instalar Reactive Forms (este módulo ya está incluido en Angular):

```bash

npm install @angular/forms
```

2. Paso 2: Estructura del Proyecto

- Tu proyecto debe tener la siguiente estructura básica:

```arduino

formulario-registro/
│
├── src/
│   ├── app/
│   │   ├── registro-reactivo/
│   │   │   ├── registro-reactivo.component.ts
│   │   │   ├── registro-reactivo.component.html
│   │   │   ├── registro-reactivo.component.css
│   │   │   └── registro-reactivo.component.spec.ts
│   │   │
│   │   ├── registro-plantilla/
│   │   │   ├── registro-plantilla.component.ts
│   │   │   ├── registro-plantilla.component.html
│   │   │   ├── registro-plantilla.component.css
│   │   │   └── registro-plantilla.component.spec.ts
│   │   │
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.module.ts
│   │   └── ...
│   └── ...
└── ...
```


3. Paso 3: Crear Componentes

- Crear el componente para el formulario reactivo:

```bash

ng generate component registro-reactivo
```

- Crear el componente para el formulario basado en plantillas:

```bash

ng generate component registro-plantilla
```

4. Paso 4: Implementar el Formulario Reactivo

- En registro-reactivo.component.ts:

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-registro-reactivo',
  templateUrl: './registro-reactivo.component.html',
})
export class RegistroReactivoComponent {
  miFormulario: FormGroup;

  constructor(private fb: FormBuilder) {
    this.miFormulario = this.fb.group({
      nombre: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      contrasena: ['', [Validators.required, Validators.minLength(6)]],
    });
  }

  onSubmit() {
    console.log('Formulario Reactivo enviado:', this.miFormulario.value);
  }
}

```

- En registro-reactivo.component.html:

```html

<form [formGroup]="miFormulario" (ngSubmit)="onSubmit()">
  <label for="nombre">Nombre:</label>
  <input id="nombre" formControlName="nombre">
  <div *ngIf="miFormulario.get('nombre').invalid && miFormulario.get('nombre').touched">
    <small>El nombre es requerido.</small>
  </div>

  <label for="email">Email:</label>
  <input id="email" formControlName="email">
  <div *ngIf="miFormulario.get('email').invalid && (miFormulario.get('email').touched || miFormulario.get('email').dirty)">
    <small *ngIf="miFormulario.get('email').errors?.required">El email es requerido.</small>
    <small *ngIf="miFormulario.get('email').errors?.email">El formato del email es inválido.</small>
  </div>

  <label for="contrasena">Contraseña:</label>
  <input id="contrasena" type="password" formControlName="contrasena">
  <div *ngIf="miFormulario.get('contrasena').invalid && miFormulario.get('contrasena').touched">
    <small *ngIf="miFormulario.get('contrasena').errors?.required">La contraseña es requerida.</small>
    <small *ngIf="miFormulario.get('contrasena').errors?.minlength">La contraseña debe tener al menos 6 caracteres.</small>
  </div>

  <button type="submit" [disabled]="miFormulario.invalid">Registrar</button>
</form>

```

5. Paso 5: Implementar el Formulario Basado en Plantillas

- En registro-plantilla.component.ts:

```typescript

import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-registro-plantilla',
  templateUrl: './registro-plantilla.component.html',
})
export class RegistroPlantillaComponent {
  onSubmit(form: NgForm) {
    console.log('Formulario de Plantilla enviado:', form.value);
  }
}
```

- En registro-plantilla.component.html:

```html

<form #miFormulario="ngForm" (ngSubmit)="onSubmit(miFormulario)">
  <label for="nombre">Nombre:</label>
  <input type="text" id="nombre" name="nombre" ngModel required>
  <div *ngIf="miFormulario.controls.nombre?.invalid && miFormulario.controls.nombre?.touched">
    <small>El nombre es requerido.</small>
  </div>

  <label for="email">Email:</label>
  <input type="email" id="email" name="email" ngModel required email>
  <div *ngIf="miFormulario.controls.email?.invalid && (miFormulario.controls.email?.touched || miFormulario.controls.email?.dirty)">
    <small *ngIf="miFormulario.controls.email?.errors?.required">El email es requerido.</small>
    <small *ngIf="miFormulario.controls.email?.errors?.email">El formato del email es inválido.</small>
  </div>

  <label for="contrasena">Contraseña:</label>
  <input type="password" id="contrasena" name="contrasena" ngModel required minlength="6">
  <div *ngIf="miFormulario.controls.contrasena?.invalid && miFormulario.controls.contrasena?.touched">
    <small *ngIf="miFormulario.controls.contrasena?.errors?.required">La contraseña es requerida.</small>
    <small *ngIf="miFormulario.controls.contrasena?.errors?.minlength">La contraseña debe tener al menos 6 caracteres.</small>
  </div>

  <button type="submit" [disabled]="miFormulario.invalid">Registrar</button>
</form>
```

6. Paso 6: Modificar el Módulo Principal

- En app.module.ts, asegúrate de importar los módulos necesarios:

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { FormsModule } from '@angular/forms'; // Importar para formularios basados en plantillas
import { AppComponent } from './app.component';
import { RegistroReactivoComponent } from './registro-reactivo/registro-reactivo.component';
import { RegistroPlantillaComponent } from './registro-plantilla/registro-plantilla.component';

@NgModule({
  declarations: [
    AppComponent,
    RegistroReactivoComponent,
    RegistroPlantillaComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    FormsModule // Añadir aquí
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

7. Paso 7: Integrar en la Aplicación

- En app.component.html, agrega ambos componentes para probarlos:

```html

<h1>Formulario de Registro</h1>

<h2>Registro con Formulario Reactivo</h2>
<app-registro-reactivo></app-registro-reactivo>

<h2>Registro con Formulario Basado en Plantillas</h2>
<app-registro-plantilla></app-registro-plantilla>

```
8. Paso 8: Ejecutar la Aplicación

- Finalmente, ejecuta tu aplicación para ver ambos formularios en acción:

```bash

ng serve
```

- Abre tu navegador y navega a http://localhost:4200/.
