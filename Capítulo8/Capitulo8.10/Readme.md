## 8.10 Métodos POST y PUT en Angular

- *Configuración Inicial*

- Antes de comenzar, asegúrate de haber configurado el HttpClientModule y de tener un servicio para interactuar con tu API. Si no lo has hecho, revisa el contenido anterior sobre Peticiones HTTP en Angular (REST).

- *Creación de un Servicio para Manejar POST y PUT*

- Vamos a extender el servicio que creamos previamente (product.service.ts) para incluir métodos para agregar y actualizar productos.

1. Paso 1: Implementar el Método POST

- El método POST se utiliza para enviar datos a la API y crear un nuevo recurso.

```typescript

// product.service.ts

createProduct(product: Product): Observable<Product> {
  return this.http.post<Product>(this.apiUrl, product).pipe(
    catchError(this.handleError)
  );
}
```

2. Paso 2: Implementar el Método PUT

- El método PUT se utiliza para actualizar un recurso existente. Generalmente, se debe especificar el ID del recurso que se va a actualizar.

```typescript

// product.service.ts

updateProduct(id: number, product: Product): Observable<Product> {
  return this.http.put<Product>(`${this.apiUrl}/${id}`, product).pipe(
    catchError(this.handleError)
  );
}
```

- *Uso de POST y PUT en Componentes*

- Ahora que tenemos los métodos en nuestro servicio, vamos a implementar la lógica para utilizarlos en un componente.

1. Paso 1: Crear un Componente para Agregar Productos

- Genera un nuevo componente llamado product-add.

```bash

ng generate component components/product-add
```

2. Paso 2: Implementar la Lógica del Componente para el Método POST

- En el componente product-add.component.ts, inyecta el servicio y utiliza el método createProduct para enviar un nuevo producto.

```typescript

import { Component } from '@angular/core';
import { ProductService } from '../services/product.service';
import { Product } from '../services/product.service';

@Component({
  selector: 'app-product-add',
  template: `
    <h2>Agregar Producto</h2>
    <form (ngSubmit)="onSubmit()">
      <label for="name">Nombre:</label>
      <input type="text" id="name" [(ngModel)]="product.name" name="name" required>
      
      <label for="description">Descripción:</label>
      <textarea id="description" [(ngModel)]="product.description" name="description" required></textarea>

      <label for="price">Precio:</label>
      <input type="number" id="price" [(ngModel)]="product.price" name="price" required>

      <button type="submit">Agregar</button>
    </form>
  `
})
export class ProductAddComponent {
  product: Product = { id: 0, name: '', description: '', price: 0 };

  constructor(private productService: ProductService) {}

  onSubmit() {
    this.productService.createProduct(this.product).subscribe({
      next: (data) => {
        console.log('Producto agregado:', data);
        // Aquí puedes agregar lógica para redirigir o mostrar un mensaje
      },
      error: (err) => {
        console.error('Error al agregar producto:', err);
      }
    });
  }
}
```

3. Paso 3: Crear un Componente para Actualizar Productos

- Genera un nuevo componente llamado product-edit.

```bash

ng generate component components/product-edit
```

4. Paso 4: Implementar la Lógica del Componente para el Método PUT

- En el componente product-edit.component.ts, inyecta el servicio y utiliza el método updateProduct para actualizar un producto existente.

```typescript

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { ProductService } from '../services/product.service';
import { Product } from '../services/product.service';

@Component({
  selector: 'app-product-edit',
  template: `
    <h2>Editar Producto</h2>
    <form (ngSubmit)="onSubmit()">
      <label for="name">Nombre:</label>
      <input type="text" id="name" [(ngModel)]="product.name" name="name" required>
      
      <label for="description">Descripción:</label>
      <textarea id="description" [(ngModel)]="product.description" name="description" required></textarea>

      <label for="price">Precio:</label>
      <input type="number" id="price" [(ngModel)]="product.price" name="price" required>

      <button type="submit">Actualizar</button>
    </form>
  `
})
export class ProductEditComponent implements OnInit {
  product!: Product;

  constructor(
    private productService: ProductService,
    private route: ActivatedRoute
  ) {}

  ngOnInit() {
    const id = +this.route.snapshot.paramMap.get('id')!;
    this.productService.getProductById(id).subscribe(data => {
      this.product = data;
    });
  }

  onSubmit() {
    const id = this.product.id;
    this.productService.updateProduct(id, this.product).subscribe({
      next: (data) => {
        console.log('Producto actualizado:', data);
        // Aquí puedes agregar lógica para redirigir o mostrar un mensaje
      },
      error: (err) => {
        console.error('Error al actualizar producto:', err);
      }
    });
  }
}
```

