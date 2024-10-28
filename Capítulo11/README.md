## Práctica 11. Funciones asincrónicas y diferencias con funciones síncronas

## Objetivo de la práctica

- Comprender y utilizar funciones asincrónicas en JavaScript y Angular, diferenciándolas de las funciones síncronas.
- Aprender a trabajar con promesas, async/await y los conceptos de eventos y callbacks, lo que te permitirá manejar operaciones que requieren tiempo, como peticiones HTTP, de manera eficiente y sin bloquear el hilo principal de ejecución.
- Crear aplicaciones más responsivas y a gestionar mejor la concurrencia en tu código.

## Duración aproximada
- 30 minutos.

## Instrucciones

1. Función síncrona.
``` javascript

function suma(a, b) {
  return a + b;
}

console.log("Inicio");
console.log(suma(2, 3)); // Salida: 5
console.log("Fin");

```

- Resultado Esperado:


```console
Inicio
5
Fin
```

2. Funciones asincrónicas.

Promesa
```javascript

function obtenerDatos() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Datos obtenidos");
    }, 2000);
  });
}

console.log("Inicio");
obtenerDatos().then((resultado) => {
  console.log(resultado); // Salida: Datos obtenidos
});
console.log("Fin");

```

- Resultado Esperado:


```console
Inicio
Fin
Datos obtenidos
```

3. Async/Await

``` javascript

async function ejecutar() {
  console.log("Inicio");
  const resultado = await obtenerDatos();
  console.log(resultado); // Salida: Datos obtenidos
  console.log("Fin");
}

ejecutar();

```
- Resultado Esperado:



```console
Inicio
Datos obtenidos
Fin
```

## Promesas en JavaScript

1. Creación de una promesa.
- Crear una promesa utilizando el constructor Promise, que toma una función llamada "executor" con dos argumentos: resolve y reject.

```javascript

const miPromesa = new Promise((resolve, reject) => {
  const exito = true; // Simulación de una condición

  if (exito) {
    resolve("Operación exitosa");
  } else {
    reject("Error en la operación");
  }
});

```

2. Consumiendo promesas.

- Para manejar el resultado de una promesa, se utilizan los métodos .then() y .catch().

then(): Se ejecuta cuando la promesa es cumplida.
catch(): Se ejecuta cuando la promesa es rechazada.

```javascript

miPromesa
  .then((resultado) => {
    console.log(resultado); // Salida: Operación exitosa
  })
  .catch((error) => {
    console.error(error); // En caso de error
  });

```

3. Encadenamiento de promesas.

- Encadenar múltiples promesas usando then(), lo que permite realizar operaciones consecutivas basadas en el resultado de la promesa anterior.

```javascript

const promesaEncadenada = new Promise((resolve) => {
  resolve(5);
});

promesaEncadenada
  .then((resultado) => resultado * 2)
  .then((resultado) => resultado + 3)
  .then((resultadoFinal) => {
    console.log(resultadoFinal); // Salida: 13
  });
```

4. Promesas combinadas.

- A veces, es necesario ejecutar múltiples promesas en paralelo. Para esto, se pueden utilizar métodos como Promise.all() y Promise.race().

    1. Promise.all()
    - Este método toma un array de promesas y devuelve una nueva promesa que se resuelve cuando todas las promesas del array se cumplen, o se rechaza si alguna de ellas falla.

```javascript

const promesa1 = Promise.resolve(3);
const promesa2 = new Promise((resolve) => setTimeout(resolve, 100, '¡Hola!'));
const promesa3 = 42;

Promise.all([promesa1, promesa2, promesa3])
  .then((resultados) => {
    console.log(resultadost); // Salida: [3, "¡Hola!", 42]
  })
  .catch((error) => {
    console.error("Una de las promesas falló:", error);
  });

```
   2. Promise.race()
    - Este método también toma un array de promesas, pero devuelve una nueva promesa que se resuelve o se rechaza tan pronto como una de las promesas se cumple o se rechaza.

```javascript

const promesaLenta = new Promise((resolve) => setTimeout(resolve, 500, 'Lenta'));
const promesaRapida = new Promise((resolve) => setTimeout(resolve, 100, 'Rápida'));

Promise.race([promesaLenta, promesaRapida])
  .then((resultado) => {
    console.log(resultado); // Salida: "Rápida"
  });
```

5. Manejo de Errores

- Es importante manejar errores adecuadamente en promesas. Los errores pueden ser capturados usando .catch() al final de la cadena de promesas, lo que permite gestionar excepciones de manera centralizada.

```javascript

const promesaConError = new Promise((resolve, reject) => {
  reject("Hubo un error");
});

promesaConError
  .then((resultado) => {
    console.log(resultado);
  })
  .catch((error) => {
    console.error(error); // Salida: Hubo un error
  });
```


## Patrón Observer

1. Diagrama del Patrón

+------------------+               +------------------+
|      Subject     |<------------->|    Observer      |
+------------------+               +------------------+
| +attach(o:Observer)|             | +update()       |
| +detach(o:Observer)|             +------------------+
| +notify()         |
| +getState()       |
| +setState()       |
+------------------+
           |
           |
+---------------------+
|   ConcreteSubject   |
+---------------------+
| +getState()        |
| +setState()        |
+---------------------+
           |
           |
+---------------------+
|  ConcreteObserver    |
+---------------------+
| +update()           |
+---------------------+

2. Implementación del Patrón Observer en JavaScript

    1. Paso 1: Definir el sujeto.
    - Crear la clase Subject que gestiona la lista de observadores y proporciona métodos para añadir, eliminar y notificar observadores.

```javascript

class Subject {
  constructor() {
    this.observers = [];
  }

  attach(observer) {
    this.observers.push(observer);
  }

  detach(observer) {
    this.observers = this.observers.filter(obs => obs !== observer);
  }

  notify() {
    this.observers.forEach(observer => observer.update(this));
  }
}
```

   2. Paso 2: Definir los observadores.
    - Definir la clase Observer, que se encargará de manejar las actualizaciones.

```javascript

class Observer {
  update(subject) {
    console.log(`Estado actualizado: ${subject.getState()}`);
  }
}
```

   3. Paso 3: Implementar el sujeto concreto.
    - Crear una clase ConcreteSubject que extiende a Subject y añadir funcionalidad para gestionar su estado.

```javascript

class ConcreteSubject extends Subject {
  constructor() {
    super();
    this.state = 0;
  }

  getState() {
    return this.state;
  }

  setState(state) {
    this.state = state;
    this.notify(); // Notificar a los observadores sobre el cambio de estado
  }
}
```

   4. Paso 4: Implementar observadores concretos.
    - Definir una clase ConcreteObserver que implementa la lógica de reacción al cambio de estado.

```javascript

class ConcreteObserver extends Observer {
  constructor(name) {
    super();
    this.name = name;
  }

  update(subject) {
    console.log(`${this.name} ha recibido la actualización: ${subject.getState()}`);
  }
}
```

3. Uso del Patrón Observer

- Ahora se puede ver el patrón en acción:

```javascript

const subject = new ConcreteSubject();

const observerA = new ConcreteObserver('Observer A');
const observerB = new ConcreteObserver('Observer B');

subject.attach(observerA);
subject.attach(observerB);

subject.setState(1); // Salida: Observer A ha recibido la actualización: 1
                     //         Observer B ha recibido la actualización: 1

subject.detach(observerA);

subject.setState(2); // Salida: Observer B ha recibido la actualización: 2

```

4. Ventajas del Patrón Observer.

- Desacoplamiento: Los sujetos y observadores están desacoplados, lo que permite una mayor flexibilidad.
- Escalabilidad: Nuevos observadores pueden ser añadidos o eliminados sin afectar al sujeto.
- Reactividad: Permite la creación de sistemas reactivos donde las actualizaciones se propagan automáticamente.


5. Desventajas del Patrón Observer.

- Sobrecarga de Notificaciones: Si hay muchos observadores, el sujeto puede tener que notificar a muchos objetos, lo que puede ser costoso.
- Complejidad: La gestión de la relación entre sujetos y observadores puede añadir complejidad a la implementación.




## Fundamentos de la Programación Reactiva

1. Observable
- Un observable es la pieza central de la programación reactiva. Permite a los desarrolladores definir flujos de datos que pueden ser observados. En JavaScript, bibliotecas como RxJS ofrecen implementaciones de observables.

- Creación de un Observable.
```javascript

import { Observable } from 'rxjs';

const miObservable = new Observable(subscriber => {
  subscriber.next('Hola');
  subscriber.next('Mundo');
  subscriber.complete();
});

miObservable.subscribe({
  next(x) { console.log(x); },
  complete() { console.log('Completado'); }
});

```
- Salida Esperada:


```consola
Hola
Mundo
Completado
```

2. Suscripciones.
- Una vez que un observable es creado, se pueden hacer suscripciones para recibir datos.

- Suscripción
```javascript

const subscription = miObservable.subscribe({
  next(x) { console.log(x); }
});

// Para cancelar la suscripción
subscription.unsubscribe();
```

## peradores Comunes en Programación Reactiva

- RxJS incluye una amplia variedad de operadores que permiten manipular flujos de datos.

1. map
- Transformar los datos emitidos por un observable.

```javascript

import { map } from 'rxjs/operators';

const numeros = [1, 2, 3];
const numerosObservable = from(numeros).pipe(
  map(num => num * 2)
);

numerosObservable.subscribe(console.log); // Salida: 2, 4, 6

```

2. filter
- Filtrar los datos emitidos según una condición.

```javascript

import { filter } from 'rxjs/operators';

const numerosFiltrados = from(numeros).pipe(
  filter(num => num > 1)
);

numerosFiltrados.subscribe(console.log); // Salida: 2, 3
```

3. combineLatest
- Combinar múltiples observables y emitir el último valor de cada uno.

```javascript

import { combineLatest } from 'rxjs';

const observable1 = of(1, 2, 3);
const observable2 = of('A', 'B', 'C');

combineLatest([observable1, observable2]).subscribe(console.log);
// Salida: [3, "C"] (últimos valores de ambos observables)

```

## RxJS y Observables

1. Usando new Observable()
- Este método permite definir un observable desde cero.

```javascript

import { Observable } from 'rxjs';

const miObservable = new Observable(subscriber => {
  subscriber.next('Hola');
  subscriber.next('Mundo');
  subscriber.complete();
});

```

2. Usando métodos de creación.
- RxJS ofrece métodos de creación como of, from, interval, y timer.

- *of*: Crear un observable a partir de valores.

```javascript

import { of } from 'rxjs';

const numeros$ = of(1, 2, 3, 4, 5);
```

- *from*: Convertir una colección iterable (como un array) en un observable.

```javascript

import { from } from 'rxjs';

const array$ = from([10, 20, 30]);
```

- *interval*: Crear un observable que emita un número secuencial cada cierto tiempo.

```javascript

import { interval } from 'rxjs';

const contador$ = interval(1000); // Emite un número cada segundo
```

3. Suscripciones.

- Para recibir datos de un observable, se debe realizar una suscripción. Cuando un observador se suscribe, el observable comienza a emitir datos.

```javascript

const subscription = miObservable.subscribe({
  next(x) { console.log(x); },
  complete() { console.log('Completado'); }
});

// Para cancelar la suscripción
subscription.unsubscribe();

```

## Operadores de RxJS

1. Transformación de datos.

- *map*: Transforma los valores emitidos por un observable.

```javascript

import { map } from 'rxjs/operators';

numeros$.pipe(
  map(num => num * 2)
).subscribe(console.log); // Salida: 2, 4, 6, 8, 10

```

2. Filtrado de datos.

- *filter*: Filtra los valores emitidos según una condición.

```javascript

import { filter } from 'rxjs/operators';

numeros$.pipe(
  filter(num => num > 2)
).subscribe(console.log); // Salida: 3, 4, 5
```

3. Combinación de observables

- *merge*: Combina múltiples observables en uno solo.

```javascript

import { merge } from 'rxjs';

const obs1$ = of('A', 'B');
const obs2$ = of('C', 'D');

merge(obs1$, obs2$).subscribe(console.log); // Salida: A, B, C, D
```

## Ejercicio practico

- Buscador en tiempo real en Angular.

1. Configuración del proyecto.

- Se asumira que ya hay un proyecto Angular creado. Si no, puedes crear uno usando Angular CLI:

```bash

ng new buscador-app
cd buscador-app
ng serve

```

2. Instalación de dependencias.

- Asegurarse de tener RxJS instalado (viene incluido con Angular por defecto).

3. Creación del servicio de búsqueda.

- Crear un servicio que simule la búsqueda de datos. Este servicio se encargará de hacer "búsquedas" en un conjunto de datos estáticos.

- Crear un servicio llamado api.service.ts:

```typescript

import { Injectable } from '@angular/core';
import { of } from 'rxjs';
import { delay } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private datos = ['Angular', 'React', 'Vue', 'Svelte', 'Ember'];

  buscar(termino: string) {
    const resultados = this.datos.filter(item =>
      item.toLowerCase().includes(termino.toLowerCase())
    );
    return of(resultados).pipe(delay(500)); // Simula un retraso en la respuesta
  }
}
```

4. Creación del componente de búsqueda

- Crear un componente que utilizará el servicio para buscar resultados a medida que el usuario escribe.

Crear un componente llamado busqueda.component.ts:

```typescript

import { Component, OnInit } from '@angular/core';
import { FormControl } from '@angular/forms';
import { debounceTime, switchMap } from 'rxjs/operators';
import { ApiService } from './api.service';

@Component({
  selector: 'app-busqueda',
  template: `
    <input [formControl]="searchControl" placeholder="Buscar..."/>
    <ul>
      <li *ngFor="let resultado of resultados">{{ resultado }}</li>
    </ul>
  `,
  styles: [`
    input {
      margin: 20px 0;
      padding: 10px;
      width: 300px;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      padding: 5px 0;
    }
  `]
})
export class BusquedaComponent implements OnInit {
  searchControl = new FormControl();
  resultados: string[] = [];

  constructor(private apiService: ApiService) {}

  ngOnInit() {
    this.searchControl.valueChanges.pipe(
      debounceTime(300), // Espera 300ms antes de buscar
      switchMap(value => this.apiService.buscar(value)) // Llama al servicio de búsqueda
    ).subscribe(resultados => {
      this.resultados = resultados; // Actualiza los resultados en la vista
    });
  }
}

```

5. Actualización del módulo.

- Asegurarse de haber importado importado ReactiveFormsModule en el módulo principal (app.module.ts):

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { BusquedaComponent } from './busqueda.component'; // Importa el componente

@NgModule({
  declarations: [
    AppComponent,
    BusquedaComponent // Declara el componente
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule // Agrega el módulo de formularios reactivos
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

6. Integración en la plantilla principal.

- Integrar el componente de búsqueda en la plantilla principal (app.component.html):

```html

<h1>Buscador en Tiempo Real</h1>
<app-busqueda></app-busqueda>
```

7. Ejecución del Proyecto.

- Ahora, puedes ejecutar el proyecto usando:

```bash

ng serve

```

- Abrir el navegador en http://localhost:4200 y deberías ver un campo de búsqueda. A medida que escribes, el componente hará una búsqueda y mostrará los resultados de forma reactiva, actualizándose después de un retraso simulado.







