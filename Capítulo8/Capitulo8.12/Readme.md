## Capítulo 8.12. Método DELETE para Eliminar un registro en Angular

- *Configuración inicial*

- Antes de comenzar, asegurarse de tener un servicio que maneje las peticiones HTTP y que esté configurado el módulo HttpClientModule. Si necesitas repasar estos pasos, consulta el contenido anterior sobre Peticiones HTTP en Angular (REST).

- *Creación de un servicio para manejar DELETE*

- Vamos a extender nuestro servicio product.service.ts para incluir un método que permita eliminar un producto utilizando el método DELETE.

1. Paso 1: Implementar el método DELETE.

- Agregar el método deleteProduct al servicio:

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

  // Método para eliminar un producto por ID
  deleteProduct(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`).pipe(
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

- *Uso del método DELETE en componentes*

- Ahora que tenemos el método DELETE implementado en el servicio, veamos cómo utilizarlo en un componente para permitir la eliminación de productos.

1. Paso 1: Actualizar el componente de lista de productos.

- Modificar el componente product-list.component.ts para incluir un botón que permita eliminar productos.

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
        <button (click)="deleteProduct(product.id)">Eliminar</button>
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
    this.loadProducts();
  }

  loadProducts() {
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

  deleteProduct(id: number) {
    if (confirm('¿Estás seguro de que deseas eliminar este producto?')) {
      this.productService.deleteProduct(id).subscribe({
        next: () => {
          this.products = this.products.filter(product => product.id !== id);
          console.log('Producto eliminado:', id);
        },
        error: (err) => {
          this.errorMessage = 'Error al eliminar el producto.';
          console.error('Error al eliminar producto:', err);
        }
      });
    }
  }
}
```

- *Explicación del método DELETE*

- *Confirmación*: Al hacer clic en el botón "Eliminar", se muestra un cuadro de diálogo de confirmación. Esto ayuda a prevenir eliminaciones accidentales.

- *Llamada al Servicio*: Si el usuario confirma la eliminación, se llama al método deleteProduct del servicio, pasando el ID del producto.

- *Actualización de la Lista*: Si la eliminación es exitosa, se actualiza la lista de productos filtrando el producto eliminado.

- *Manejo de Errores*: Si ocurre un error durante la eliminación, se muestra un mensaje de error.

