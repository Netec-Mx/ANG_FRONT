## Peticiones HTTP en Angular (REST)

- *Instalación de HttpClientModule*

- Antes de comenzar, asegúrate de que el módulo HttpClientModule esté importado en tu aplicación. Esto se hace generalmente en el archivo app.module.ts.

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule // Importa el módulo aquí
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- *Creación de un Servicio HTTP*

- Para manejar las peticiones HTTP, es recomendable crear un servicio Angular. Los servicios permiten mantener la lógica de negocio separada de los componentes.

1. *Paso 1: Generar un Servicio*

- Utiliza Angular CLI para crear un servicio. Por ejemplo, si queremos interactuar con una API de productos, podemos crear un servicio llamado product.service.ts.

```bash

ng generate service services/product
```

2. *Paso 2: Implementar el Servicio*

- En product.service.ts, inyecta el HttpClient y define métodos para realizar las peticiones.

```typescript

import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class ProductService {
  private apiUrl = 'https://api.example.com/products'; // URL de la API

  constructor(private http: HttpClient) {}

  // Método para obtener todos los productos
  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl).pipe(
      catchError(this.handleError)
    );
  }

  // Método para obtener un producto por ID
  getProductById(id: number): Observable<Product> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`).pipe(
      catchError(this.handleError)
    );
  }

  // Método para crear un nuevo producto
  createProduct(product: Product): Observable<Product> {
    return this.http.post<Product>(this.apiUrl, product).pipe(
      catchError(this.handleError)
    );
  }

  // Método para manejar errores
  private handleError(error: HttpErrorResponse) {
    // Manejo de errores: mostrar mensaje y devolver un observable con el error
    console.error('Ocurrió un error:', error);
    return throwError('Algo salió mal; intenta de nuevo más tarde.');
  }
}
```

- *Definición de la Interfaz de Producto*

- Es útil definir una interfaz para los productos para tener una estructura clara.

```typescript

export interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
}
```

- *Realizando Peticiones en Componentes*

- Ahora que tenemos el servicio, podemos inyectarlo en un componente para realizar las peticiones.

1. Paso 1: Generar un Componente

- Crea un componente que muestre los productos.

```bash

ng generate component components/product-list
```

2. Paso 2: Implementar la Lógica en el Componente

- Inyecta el servicio en el constructor del componente y utiliza el método getProducts() para obtener la lista de productos.

```typescript

import { Component, OnInit } from '@angular/core';
import { ProductService } from '../services/product.service';
import { Product } from '../services/product.service';

@Component({
  selector: 'app-product-list',
  template: `
    <h2>Lista de Productos</h2>
    <ul>
      <li *ngFor="let product of products">
        {{ product.name }} - {{ product.price | currency }}
      </li>
    </ul>
  `
})
export class ProductListComponent implements OnInit {
  products: Product[] = [];

  constructor(private productService: ProductService) {}

  ngOnInit() {
    this.productService.getProducts().subscribe({
      next: (data) => {
        this.products = data;
      },
      error: (err) => {
        console.error('Error al obtener productos:', err);
      }
    });
  }
}
```

- *Manejo de Errores*

- El manejo de errores es crucial para ofrecer una buena experiencia de usuario. En el método handleError() del servicio, se pueden realizar acciones como mostrar mensajes en la interfaz o registrar errores en un sistema externo.

- *Ejemplo de Manejo de Errores en el Componente*

- Puedes mostrar un mensaje de error en el componente si la petición falla.

```typescript

errorMessage: string | null = null;

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
```

