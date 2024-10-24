# 8.15. Uso de Interceptores en Angular

- *¿Qué es un Interceptor?*

- Un interceptor es un servicio que implementa la interfaz HttpInterceptor y que se registra en el módulo AppModule. Cada vez que se realiza una solicitud HTTP, Angular pasa esa solicitud a través de todos los interceptores registrados en la aplicación. Esto permite realizar acciones antes de que la solicitud llegue al servidor y después de recibir la respuesta.

- *Creación de un interceptor*

1. Paso 1: Generar un interceptor.

- Utilizar Angular CLI para generar un interceptor. Por ejemplo, podemos crear un interceptor de autenticación.

```bash

ng generate interceptor services/auth
```

2. Paso 2: Implementar el interceptor.

- En el archivo auth.interceptor.ts, implementar el interceptor para agregar un token de autenticación a las solicitudes HTTP.

```typescript

import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Obtener el token de autenticación (puede ser de un servicio o del almacenamiento local)
    const token = localStorage.getItem('authToken');

    // Clonar la solicitud para agregar el nuevo encabezado
    if (token) {
      const clonedRequest = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
      return next.handle(clonedRequest); // Pasar la solicitud clonada al siguiente manejador
    }
    return next.handle(req); // Pasar la solicitud original si no hay token
  }
}
```

3. Paso 3: Registrar el interceptor.

- Para que Angular reconozca el interceptor, debes registrarlo en el módulo principal (app.module.ts).

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AppComponent } from './app.component';
import { AuthInterceptor } from './services/auth.interceptor';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true } // Registrar el interceptor
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- *Uso de interceptores para manejo de errores*

- Además de la autenticación, los interceptores son ideales para manejar errores de forma global. Puedes crear un interceptor que capture errores de las respuestas HTTP.

1. Paso 1: crear un interceptor de errores.

- Generar un nuevo interceptor:

```bash

ng generate interceptor services/error
```

2. Paso 2: Implementar el interceptor de errores.

- En error.interceptor.ts, implementar el manejo de errores.

```typescript

import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        let errorMessage = 'Ocurrió un error desconocido';
        if (error.error instanceof ErrorEvent) {
          errorMessage = `Error: ${error.error.message}`;
        } else {
          errorMessage = `Código de error: ${error.status}\nMensaje: ${error.message}`;
        }
        console.error(errorMessage);
        return throwError(errorMessage);
      })
    );
  }
}
```

3. Paso 3: Registrar el interceptor de errores.

- Asegurarse de registrar este interceptor en el mismo lugar que el interceptor de autenticación.

```typescript

providers: [
  { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true }
],
```

