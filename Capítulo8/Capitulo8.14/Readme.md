# Capítulo 8.14. Manejo de errores en Angular

- *Importancia del manejo de errores*

- Un manejo adecuado de errores permite:

    - Proporcionar feedback claro al usuario.
    - Evitar que la aplicación se bloquee.
    - Facilitar la identificación de problemas en el código.
    - Mejorar la seguridad de la aplicación.


- *Estrategias para manejar errores*

1. Manejo de errores en servicios.

- Al realizar peticiones HTTP, los errores pueden ocurrir debido a problemas de red, respuestas no válidas del servidor, o errores de autenticación. Angular proporciona el objeto HttpErrorResponse para gestionar estos errores.

- *Ejemplo de manejo de errores en un servicio*

```typescript

import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class ProductService {
  private apiUrl = 'https://api.example.com/products';

  constructor(private http: HttpClient) {}

  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl).pipe(
      catchError(this.handleError)
    );
  }

  private handleError(error: HttpErrorResponse) {
    let errorMessage = 'Ocurrió un error desconocido';
    if (error.error instanceof ErrorEvent) {
      // Errores del lado del cliente
      errorMessage = `Error: ${error.error.message}`;
    } else {
      // Errores del lado del servidor
      errorMessage = `Código de error: ${error.status}\nMensaje: ${error.message}`;
    }
    console.error(errorMessage);
    return throwError(errorMessage);
  }
}
```

2. Manejo de errores en componentes.

- Los componentes que consumen servicios también deben manejar los errores. Esto se puede hacer suscribiendo a los observables y manejando el error en la suscripción.

- *Ejemplo de manejo de errores en un componente*

```typescript

import { Component, OnInit } from '@angular/core';
import { ProductService, Product } from '../services/product.service';

@Component({
  selector: 'app-product-list',
  template: `
    <h2>Lista de Productos</h2>
    <ul>
      <li *ngFor="let product of products">
        {{ product.name }} - {{ product.price | currency }}
      </li>
    </ul>
    <div *ngIf="errorMessage">{{ errorMessage }}</div>
  `
})
export class ProductListComponent implements OnInit {
  products: Product[] = [];
  errorMessage: string | null = null;

  constructor(private productService: ProductService) {}

  ngOnInit() {
    this.productService.getProducts().subscribe({
      next: (data) => {
        this.products = data;
      },
      error: (err) => {
        this.errorMessage = 'No se pudieron cargar los productos.';
        console.error('Error al obtener productos:', err);
      }
    });
  }
}
```

3. Uso de interceptores para manejo global de errores.

- Los interceptores permiten manejar errores de manera centralizada, lo que es útil para implementar lógicas de manejo de errores comunes en toda la aplicación.

- *Ejemplo de un interceptor de errores*

- Primero, generar un interceptor:

```bash

ng generate interceptor services/error

```

- Luego, implementar el interceptor en error.interceptor.ts:

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

4. Registro de errores.

- Además de mostrar mensajes de error al usuario, es importante registrar los errores para análisis futuros. Puedes integrar un servicio de logging que envíe los errores a un servidor o sistema de seguimiento de errores.

- *Ejemplo de servicio de registro*

```typescript

@Injectable({
  providedIn: 'root'
})
export class LoggingService {
  logError(message: string) {
    // Aquí puedes enviar el mensaje a un servidor
    console.error(message);
  }
}
```

- *Integración del servicio de registro*

- Puedes llamar a este servicio en el manejo de errores del servicio o del interceptor:

```typescript

constructor(private loggingService: LoggingService) {}

private handleError(error: HttpErrorResponse) {
  let errorMessage = 'Ocurrió un error desconocido';
  // Lógica para construir el mensaje de error
  this.loggingService.logError(errorMessage);
  return throwError(errorMessage);
}

```
