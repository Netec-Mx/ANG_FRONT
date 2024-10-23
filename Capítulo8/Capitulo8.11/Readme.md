## 8.11. Método GET para recuperar información en Angular

- *Configuración Inicial*

- Antes de empezar, asegúrate de que el módulo HttpClientModule esté importado en tu aplicación y que tengas un servicio para interactuar con una API. Si necesitas repasar esta configuración, consulta el contenido anterior sobre Peticiones HTTP en Angular (REST).

- *Creación de un Servicio para Manejar GET*

- Vamos a crear o extender un servicio que se encargue de las peticiones GET. Si estás trabajando con productos, por ejemplo, podemos llamarlo product.service.ts.

1. Paso 1: Implementar el Método GET

- En el archivo product.service.ts, implementa el método GET para recuperar todos los productos y un método adicional para obtener un producto específico por ID.

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

  // Método para obtener un producto por ID
  getProductById(id: number): Observable<Product> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`).pipe(
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

- *Uso del Método GET en Componentes*

- Ahora que tenemos los métodos para realizar las peticiones GET en nuestro servicio, vamos a implementarlos en un componente para mostrar la lista de productos.

1. Paso 1: Crear un Componente para Listar Productos

- Genera un nuevo componente llamado product-list.

```bash

ng generate component components/product-lis
```

2. Paso 2: Implementar la Lógica del Componente

- En product-list.component.ts, inyecta el servicio y utiliza el método getProducts() para recuperar la lista de productos.

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
        <button (click)="viewDetails(product.id)">Ver Detalles</button>
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

  viewDetails(id: number) {
    // Aquí podrías implementar la lógica para ver los detalles de un producto
    console.log('Ver detalles del producto con ID:', id);
  }
}
```

- *Manejo de Errores*

- El manejo de errores es crucial para proporcionar una buena experiencia al usuario. En el ejemplo anterior, si ocurre un error al recuperar la lista de productos, se mostrará un mensaje de error en el componente.

