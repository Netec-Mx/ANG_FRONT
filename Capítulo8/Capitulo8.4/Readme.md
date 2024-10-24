# Capítulo 8.4. Navegación con el objeto Router en Angular

- *Inyección del objeto Router*

- Para utilizar el objeto Router, primero debes inyectarlo en el componente. Esto se hace a través del constructor del componente.

```typescript

import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent {
  constructor(private router: Router) {}
}

```

- *Navegación Programática*

- Una de las funcionalidades más útiles del objeto Router es la capacidad de navegar programáticamente a diferentes rutas. Esto se realiza utilizando el método navigate().

- *Navegación Simple*
```typescript

goToAbout() {
  this.router.navigate(['/about']);
}
```

- *Navegación con Parámetros*

- Si necesitas navegar a una ruta que incluye parámetros, puedes pasarlos como un array:

```typescript

goToUserDetail(userId: number) {
  this.router.navigate(['/user', userId]);
}
```

- *Navegación con Query Parameters*

- También puedes añadir parámetros de consulta a las rutas utilizando un objeto adicional:

```typescript

goToProducts(category: string) {
  this.router.navigate(['/products'], { queryParams: { category } });
}
```
- *Navegación condicional*

- El objeto Router también permite implementar lógica de navegación condicional. Por ejemplo, puedes redirigir a los usuarios a diferentes rutas según su estado de autenticación:

```typescript

if (isAuthenticated) {
  this.router.navigate(['/dashboard']);
} else {
  this.router.navigate(['/login']);
}
```

- *Retorno a la ruta anterior*

- Para regresar a la ruta anterior, utilizar el método navigate() con el valor ['/'] o, si necesitas volver exactamente a la última ruta, usar el servicio Location de Angular:

```typescript

import { Location } from '@angular/common';

constructor(private location: Location) {}

goBack() {
  this.location.back();
}
```

- *Navegación en servicios*

- Además de los componentes, también puedes usar el objeto Router en servicios para realizar redirecciones. Esto es útil, por ejemplo, para manejar la lógica de autenticación en un servicio de usuario.

```typescript

import { Injectable } from '@angular/core';
import { Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  constructor(private router: Router) {}

  login() {
    // Lógica de autenticación
    this.router.navigate(['/dashboard']);
  }

  logout() {
    this.router.navigate(['/login']);
  }
}
```

