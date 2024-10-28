# Práctica 12. Protegiendo tu aplicación de amenazas comunes: Seguridad en Angular

## Objetivo de la práctica

- Implementar prácticas de seguridad en aplicaciones Angular. 
- Aprender sobre los principales riesgos de seguridad, como la inyección de código (XSS), la falsificación de solicitudes (CSRF) y el manejo seguro de datos.
- Entender cómo utilizar herramientas y técnicas como la sanitización de entradas, la autenticación y autorización, y el manejo de tokens, para proteger tu aplicación y los datos de los usuarios de amenazas comunes.

## Duración aproximada
- 30 minutos.

## Instrucciones
1. Principales amenazas de seguridad.

- Antes de explorar las características de seguridad de Angular, es importante entender algunas de las amenazas comunes que pueden afectar a las aplicaciones web:

*Cross-Site Scripting (XSS)*: 
- Esta vulnerabilidad permite a un atacante inyectar scripts maliciosos en las páginas web vistas por otros usuarios. Esto puede resultar en robo de datos, suplantación de identidad, etc.

*Cross-Site Request Forgery (CSRF)*: 
- Un ataque en el que un usuario autenticado es engañado para ejecutar acciones no deseadas en una aplicación web en la que está autenticado.

*Inyección de Código*: 
- Implica la inserción de código malicioso a través de formularios o parámetros, lo que puede comprometer la seguridad del servidor.

2. Protección contra XSS.

- Angular tiene un enfoque sólido para mitigar las vulnerabilidades XSS a través del data binding y la sanitización.

    1. Data binding seguro.
    - Angular utiliza el data binding para mantener los datos en la vista y el modelo sincronizados. Este enfoque ayuda a prevenir XSS, ya que Angular escapa automáticamente los valores interpolados en las plantillas.

```html

<!-- Seguro: Angular escapa los valores por defecto -->
<div>{{ userInput }}</div>
```

   2. Sanitización.
    - Angular proporciona el servicio DomSanitizer, que permite a los desarrolladores sanitizar el contenido que puede incluir HTML, URL, estilos, entre otros.

```typescript

import { Component } from '@angular/core';
import { DomSanitizer } from '@angular/platform-browser';

@Component({
  selector: 'app-example',
  template: `<div [innerHTML]="sanitizedContent"></div>`
})
export class ExampleComponent {
  sanitizedContent: any;

  constructor(private sanitizer: DomSanitizer) {
    const unsafeHtml = '<script>alert("XSS")</script>';
    this.sanitizedContent = this.sanitizer.bypassSecurityTrustHtml(unsafeHtml);
  }
}
```
- Nota: El uso de bypassSecurityTrust... debe hacerse con precaución, ya que puede introducir vulnerabilidades si se usa incorrectamente.


3. Protección contra CSRF.

- Para proteger contra ataques CSRF, Angular utiliza un mecanismo de tokens. La biblioteca Angular realiza automáticamente el envío de un token de CSRF en cada solicitud HTTP. Esto se gestiona en la configuración del servidor y requiere que el servidor valide el token.

    1. Implementación de CSRF en Angular.
    - Al realizar solicitudes HTTP con HttpClient, Angular incluye automáticamente el token CSRF en las cabeceras.

```typescript

import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  constructor(private http: HttpClient) {}

  realizarPeticion() {
    return this.http.get('/api/endpoint'); // El token CSRF se enviará automáticamente
  }
}
```

4. Autenticación y autorización.

- La seguridad también implica garantizar que solo los usuarios autenticados y autorizados tengan acceso a ciertas partes de la aplicación.

    1. Autenticación.
    - Angular puede trabajar con diferentes métodos de autenticación (como JWT, OAuth, etc.). Generalmente, la autenticación se maneja a través de servicios que almacenan el token en localStorage o sessionStorage.

    2. Guardianes de rutas.
    - Los guardianes de rutas (route guards) en Angular permiten proteger las rutas, asegurándose de que solo los usuarios autenticados puedan acceder a ellas.

```typescript

import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    const isAuthenticated = // lógica de autenticación aquí

    if (!isAuthenticated) {
      this.router.navigate(['/login']);
      return false;
    }
    return true;
  }
}
```

5. Mejoras en la seguridad.

    1. Content Security Policy (CSP).
    - Implementar una política de seguridad de contenido (CSP) ayuda a mitigar XSS y otros ataques inyectados al controlar qué recursos pueden ser cargados y ejecutados.

    2. Actualizaciones y parches.
    - Es crucial mantener Angular y sus dependencias actualizadas para protegerse contra vulnerabilidades conocidas. Regularmente se lanzan actualizaciones que corrigen problemas de seguridad.


## Accesibilidad en Angular

1. Principios de la accesibilidad.

- La accesibilidad se basa en varios principios, conocidos como los principios POUR:

    - *Perceptible*: La información y los componentes de la interfaz de usuario deben ser presentados de manera que puedan ser percibidos por todos los usuarios.

    - *Operable*: Los componentes de la interfaz deben ser operables por todos los usuarios, independientemente de sus habilidades.

    - *Comprensible*: La información y la operación de la interfaz deben ser comprensibles.

    - *Robusto*: El contenido debe ser robusto para que pueda ser interpretado por una amplia variedad de agentes de usuario, incluidos los lectores de pantalla.

2. Prácticas de accesibilidad en Angular.

    1. Uso de ARIA (Accessible Rich Internet Applications).

    - ARIA proporciona un conjunto de atributos que se pueden agregar a HTML para mejorar la accesibilidad. Angular permite usar estos atributos en sus plantillas.

Ejemplo:

```html

<button aria-label="Cerrar" (click)="cerrar()">
  <span aria-hidden="true">✖</span>
</button>
```

- En este ejemplo, el atributo aria-label proporciona una descripción del botón para los lectores de pantalla.

    2. Navegación por teclado.
    - Asegurarse de que todos los elementos interactivos sean accesibles mediante el teclado. Esto incluye enlaces, botones y formularios. Utilizar las teclas de tabulación para permitir la navegación por los elementos.

Ejemplo:

```html

<a href="#" (keydown.enter)="accion()" tabindex="0">Acción</a>
```

- El uso de tabindex permite que el enlace sea accesible mediante la tecla de tabulación.

    3. Validación de formularios.

    - Angular proporciona soporte para formularios reactivos y de plantillas que incluyen la validación. Es importante informar a los usuarios sobre errores de validación de manera clara.

Ejemplo:

```html

<form [formGroup]="miFormulario">
  <input formControlName="nombre" aria-describedby="nombreAyuda">
  <div *ngIf="miFormulario.get('nombre').invalid && miFormulario.get('nombre').touched">
    <small id="nombreAyuda">El nombre es obligatorio.</small>
  </div>
</form>
```

- El uso de aria-describedby ayuda a los lectores de pantalla a conectar la entrada con su mensaje de error.

    4. Color y contraste.

    - Asegúrate de que el contraste entre el texto y el fondo sea suficiente para que los usuarios con discapacidades visuales puedan leer el contenido. Utiliza herramientas de contraste de color para verificar que cumples con las pautas de accesibilidad.

    5. Etiquetas y roles.

    - Siempre proporciona etiquetas para los elementos de entrada y utiliza roles apropiados para los componentes personalizados.

Ejemplo:

```html

<label for="email">Correo Electrónico</label>
<input type="email" id="email" aria-required="true">

```

- Aquí, el uso de label y aria-required mejora la accesibilidad del campo de entrada.

3. Uso de librerías y componentes accesibles.

- Utiliza componentes de UI que sean accesibles por defecto. Muchas bibliotecas de componentes para Angular, como Angular Material, ofrecen componentes accesibles. Asegúrate de revisar la documentación para seguir las mejores prácticas de accesibilidad.

4. Pruebas de accesibilidad.

- Es fundamental realizar pruebas de accesibilidad en tu aplicación. Utiliza herramientas como:

- *Axe*: 
- Una herramienta de auditoría de accesibilidad.

- *Lighthouse*: 
- Una herramienta integrada en Chrome que puede evaluar la accesibilidad de tu aplicación.

- *Screen Readers*: 
- Prueba la aplicación con lectores de pantalla como NVDA o JAWS para garantizar que la experiencia sea adecuada.


## Mejores prácticas sobre Property Binding en Angular

1. ¿Qué es Property Binding?

- El property binding permite enlazar propiedades de un componente a propiedades de un elemento en el template. La sintaxis utiliza corchetes [] para indicar que se está enlazando una propiedad.

Ejemplo:

```html

<img [src]="imageUrl" [alt]="imageDescription">
```

- En este ejemplo, el src y alt del elemento <img> están enlazados a propiedades del componente.

2. Mejores prácticas.

    1. Usar Property Binding en lugar de interpolación.
    - Cuando trabajes con propiedades que afectan a elementos DOM, es preferible usar property binding en lugar de interpolación, ya que el binding es más eficiente y puede prevenir problemas de rendimiento.

- Menos Eficiente:

```html

<img src="{{ imageUrl }}" alt="{{ imageDescription }}">
```

- Más Eficiente:

```html

<img [src]="imageUrl" [alt]="imageDescription">
```
    
   2. Evitar cambios frecuentes en las propiedades.

    - Minimiza el número de cambios en las propiedades enlazadas para mejorar el rendimiento. Los cambios frecuentes pueden llevar a un rendimiento deficiente debido a las múltiples actualizaciones del DOM.

- Mala práctica:

```typescript

updateImage() {
  this.imageUrl = 'new-image-url.jpg'; // Cambiar frecuentemente
}
```

- Mejor Práctica:

- Agrupa los cambios o usa un enfoque que limite las actualizaciones:

```typescript

updateImageBatch() {
  this.imageUrls = ['url1.jpg', 'url2.jpg', 'url3.jpg'];
}

```

-

   3. Utilizar el cambio de detección OnPush.
    - Si es posible, usar la estrategia de detección de cambios OnPush en los componentes para optimizar el rendimiento. Esto reduce el número de chequeos de cambios y mejora la eficiencia al enlazar propiedades.

```typescript

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ExampleComponent {
  // Propiedades
}

```

   4. Mantener el código limpio y legible.
    - Es esencial mantener el código claro y legible. Utilizar nombres de propiedades significativos y documentar las propiedades que se enlazan para facilitar la comprensión.

Ejemplo:

```typescript

// Mejor práctica: usa nombres claros
public imageUrl: string;
public imageDescription: string;

```

   5. Evitar el enlace de propiedades innecesarias.
    - No enlazar propiedades que no sean necesarias. Cada enlace agrega carga al ciclo de detección de cambios, por lo que es mejor limitar el binding a lo que realmente se necesita.

Ejemplo:

```html

<!-- Enlace innecesario -->
<div [class.active]="isActive" [style.display]="isVisible ? 'block' : 'none'"></div>
```

- En lugar de enlazar ambos, considerar la lógica de negocio para simplificar.

    6. Usar template reference variables.
    - Cuando se necesita interactuar con elementos DOM directamente, las template reference variables son una opción eficiente y legible.

Ejemplo:

```html

<input #inputElement type="text" (input)="onInput(inputElement.value)">
```

- Esto evita el uso de ViewChild en situaciones donde se necesita acceso directo al valor.

3. Ejemplos prácticos.

    1. Binding de clases y estilos.
    - El property binding también se puede usar para enlazar clases y estilos de manera dinámica.

```html

<div [ngClass]="{'active': isActive}" [style.color]="textColor"></div>
```

- Esto proporciona una forma más flexible y clara de aplicar estilos basados en la lógica del componente.


## Módulos de funciones de carga diferida en Angular

1. ¿Qué es la carga diferida?

- La carga diferida consiste en dividir la aplicación en módulos independientes que se cargan solo cuando se accede a ellos. En lugar de cargar toda la aplicación al inicio, solo se cargan los módulos necesarios. Esto se logra mediante la configuración del enrutador de Angular.

2. Beneficios de la carga diferida.

- *Mejora del Rendimiento*: Al cargar solo los módulos necesarios, se reduce el tamaño del archivo JavaScript que se envía al navegador, lo que resulta en un tiempo de carga más rápido.

- *Optimización de Recursos*: Se utilizan menos recursos en el navegador, lo que mejora la experiencia del usuario en dispositivos móviles y conexiones lentas.

- *Escalabilidad*: Permite estructurar la aplicación en módulos independientes, facilitando el mantenimiento y la expansión de la aplicación en el futuro.

3. Implementación de carga diferida.

    1. Crear un módulo
    - Primero, necesitas crear un módulo que se cargará de forma diferida. Por ejemplo, supongamos que queremos crear un módulo para una sección de administración.

```bash

ng generate module admin --route admin --module app.module
```

- Este comando generará un módulo AdminModule y configurará automáticamente las rutas en el AppRoutingModule para que se cargue de forma diferida.

    2. Configurar el enrutador.
    - En el archivo app-routing.module.ts, se establecerá una ruta que use la carga diferida:

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  },
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: '**', redirectTo: '/home' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- En este ejemplo, la ruta /admin se configura para cargar AdminModule de forma diferida. La función loadChildren utiliza la sintaxis de función de flecha para devolver una promesa que resuelve el módulo.

    3. Crear componentes en el módulo.

    - Dentro de AdminModule, puedes generar los componentes necesarios:

```bash

ng generate component admin/dashboard
ng generate component admin/users
```

- Configurar las rutas dentro de admin-routing.module.ts para que los componentes sean accesibles.

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { DashboardComponent } from './dashboard/dashboard.component';
import { UsersComponent } from './users/users.component';

const routes: Routes = [
  { path: '', component: DashboardComponent },
  { path: 'users', component: UsersComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class AdminRoutingModule { }
```
- 

    4. Consideraciones al usar carga diferida.

    *Gestión de Errores*: Es importante manejar posibles errores al cargar módulos diferidos. Puedes utilizar el manejo de errores en el loadChildren para gestionar casos en los que el módulo no se carga correctamente.

    *Prefetching*: Considerar el uso de estrategias de prefetching para anticipar la carga de ciertos módulos que el usuario podría necesitar a continuación.

    *Tamaño del Paquete*: Evaluar el tamaño de los módulos. Si un módulo es muy grande, considera dividirlo en módulos más pequeños que se carguen de forma diferida.






