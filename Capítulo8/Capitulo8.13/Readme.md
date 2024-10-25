# Capítulo 8.13. Leer APIs REST desde Angular

## Objetivo de la práctica
- Crear un servicio para leer datos.
  
## Duración aproximada
- 10 minutos.

## Instrucciones 

- *Configuración inicial*

- Antes de empezar, asegurarse de tener un proyecto Angular configurado y de haber importado el módulo HttpClientModule. Esto se hace generalmente en el archivo app.module.ts.

1. Paso 1: Importar HttpClientModule.

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
    HttpClientModule // Importar el módulo aquí
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- *Creación de un servicio para leer datos*

- Para interactuar con una API REST, es recomendable crear un servicio Angular que encapsule la lógica de las peticiones HTTP.

1. Paso 1: Generar un servicio.

- Utilizar Angular CLI para crear un servicio. Por ejemplo, si queremos leer datos de productos, podemos crear un servicio llamado product.service.ts.

```bash

ng generate service services/product
```
2. Paso 2: Implementar el Servicio

- En product.service.ts, inyectar el HttpClient y definir métodos para realizar las peticiones GET.

```typescript

import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

export interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
}

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

  // Manejo de errores
  private handleError(error: HttpErrorResponse) {
    console.error('Ocurrió un error:', error);
    return throwError('Algo salió mal; intenta de nuevo más tarde.');
  }
}
```

- *Lectura de Datos en un Componente*

- Ahora que tenemos el servicio configurado, vamos a utilizarlo en un componente para mostrar los datos.

1. Paso 1: Crear un componente.

- Generar un nuevo componente que se encargue de listar los productos.

```bash

ng generate component components/product-list
```

2. Paso 2: Implementar la lógica del componente.

- En product-list.component.ts, inyectar el servicio y utilizar el método getProducts() para recuperar la lista de productos.

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

- *Explicación del componente*

- *Inyección de servicio*: El servicio ProductService se inyecta en el constructor del componente.

- *ngOnInit*: En el método ngOnInit(), se llama al método getProducts() del servicio. Este método devuelve un observable que se suscribe para recibir los datos.

- *Manejo de datos y errores*: Si la llamada es exitosa, los datos se asignan a la propiedad products. En caso de error, se captura y muestra un mensaje.

- *Visualización de datos*

- El componente product-list usa *ngFor para iterar sobre el array de productos y mostrar cada producto en una lista.
