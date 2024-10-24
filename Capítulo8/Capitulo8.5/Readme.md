## Capítulo 8.5. Query Params en Angular

- *¿Qué son los Query Params?*
- Los query params son parte de la URL que se utilizan para enviar datos a un servidor o para definir el estado de la aplicación. Se presentan en la URL después del signo de interrogación (?). Por ejemplo:

```bash

https://example.com/products?category=electronics&sort=price
```

- En este caso, category y sort son query params.

- *Configuración de Rutas*

- No necesitas definir explícitamente los query params en la configuración de rutas. Angular permite que cualquier ruta acepte query params sin necesidad de especificarlos.

- Ejemplo de Ruta

```typescript

const routes: Routes = [
  { path: 'products', component: ProductsComponent },
];
```

- *Navegación con Query Params*

- Puedes navegar a una ruta con query params utilizando el método navigate() del objeto Router.

- Ejemplo de navegación:

```typescript

import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-product-list',
  template: `
    <button (click)="goToProducts('electronics', 'price')">Ver Productos Electrónicos</button>
  `
})
export class ProductListComponent {
  constructor(private router: Router) {}

  goToProducts(category: string, sort: string) {
    this.router.navigate(['/products'], { queryParams: { category, sort } });
  }
}
```

- *Acceso a Query Params*
- Para acceder a los query params en un componente, puedes usar el servicio ActivatedRoute. Este servicio permite obtener los parámetros de la URL en el componente donde se ha activado la ruta.

- Ejemplo de acceso
```typescript

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-products',
  template: `
    <h1>Productos</h1>
    <p>Categoría: {{ category }}</p>
    <p>Ordenar por: {{ sort }}</p>
  `
})
export class ProductsComponent implements OnInit {
  category!: string;
  sort!: string;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.route.queryParams.subscribe(params => {
      this.category = params['category'];
      this.sort = params['sort'];
    });
  }
}

```

- *Actualización de Query Params*

- Si deseas actualizar los query params sin recargar el componente, utilizar el método navigate del Router con un objeto queryParams que contenga los nuevos valores.

- Ejemplo de actualización:

```typescript

updateCategory(newCategory: string) {
  this.router.navigate([], {
    relativeTo: this.route,
    queryParams: { category: newCategory },
    queryParamsHandling: 'merge', // Mantiene los otros query params
  });
}
```

- *Eliminación de Query Params*

- Para eliminar un query param específico, simplemente se debe establecer su valor en null o undefined en el objeto queryParams.

- Ejemplo de eliminación:

```typescript

removeSortParam() {
  this.router.navigate([], {
    relativeTo: this.route,
    queryParams: { sort: null }, // Eliminará el query param 'sort'
    queryParamsHandling: 'merge', // Mantiene otros parámetros
  });
}
```

