## Capítulo 8.7. Componente de Error en Angular

- *Creación de un componente de error*

1. Generar el componente:

- Utilizar Angular CLI para generar un nuevo componente llamado ErrorComponent.

```bash

ng generate component error
```

2. Definir la plantilla del componente:

- En el archivo error.component.html, define la plantilla que se mostrará cuando ocurra un error. Puedes incluir un mensaje amigable y, opcionalmente, un botón para volver a la página anterior o a la página principal.

```html

<div class="error-container">
  <h1>¡Vaya! Ha ocurrido un error.</h1>
  <p>Lo sentimos, pero algo salió mal. Por favor, intenta de nuevo más tarde.</p>
  <a routerLink="/">Volver a la página principal</a>
</div>
```

3. Estilo del componente:

-  Agrega estilos en error.component.css para mejorar la presentación.

```css

.error-container {
  text-align: center;
  padding: 20px;
  color: #d9534f;
}
```

- *Manejo de errores en la aplicación*

1. Manejo de errores de navegación.

- Para manejar errores de navegación, configurar un catch-all route en el archivo de rutas. Esto redirigirá a los usuarios a tu ErrorComponent si intentan acceder a una ruta que no existe.

```typescript

const routes: Routes = [
  // Rutas definidas
  { path: '**', component: ErrorComponent }
];
```

2. Manejo de errores en servicios.

- Si estás realizando solicitudes HTTP y necesitas manejar errores, puedes implementar un HttpInterceptor o manejar errores directamente en el servicio.

- *Ejemplo de manejo de errores en un servicio:*

```typescript

import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  constructor(private http: HttpClient) {}

  getData() {
    return this.http.get('https://api.example.com/data')
      .pipe(
        catchError(this.handleError)
      );
  }

  private handleError(error: HttpErrorResponse) {
    // Aquí puedes realizar el manejo de errores
    console.error('Ocurrió un error:', error);
    return throwError('Algo salió mal; intenta de nuevo más tarde.');
  }
}
```

3. Redirigir a componente de error en caso de error.

- Si deseas redirigir al usuario al componente de error desde un servicio o componente en caso de que ocurra un error, puedes utilizar el Router.

- *Ejemplo de redirección:*

```typescript

import { Router } from '@angular/router';

constructor(private router: Router) {}

someMethod() {
  this.dataService.getData().subscribe({
    next: data => {
      // Manejar los datos
    },
    error: err => {
      this.router.navigate(['/error']); // Redirigir al componente de error
    }
  });
}
```

- *Mejoras adicionales*

1. Interacción con el usuario.

- Puedes mejorar el componente de error permitiendo que los usuarios informen sobre el error, proporcionando un formulario sencillo que capture detalles adicionales.

2. Registro de errores.

- Es recomendable implementar un sistema de registro de errores, ya sea en el cliente o enviando los errores a un servidor para su análisis. Puedes usar servicios como Sentry o Rollbar para rastrear y gestionar errores en producción.
