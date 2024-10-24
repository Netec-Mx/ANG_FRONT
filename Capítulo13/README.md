# Práctica 13. Pruebas unitarias en Angular: Asegurando la calidad del código

## Objetivo de la práctica

- Realizar pruebas efectivas en Angular, cubriendo métodos públicos y privados, suscripciones a observables, Pipes y servicios que interactúan con APIs. 
- Aprender a implementar pruebas unitarias utilizando herramientas como Jasmine y Karma.
- Aprender a simular (mockear) servicios para aislar componentes en las pruebas y a asegurar que tu aplicación funcione correctamente y cumpla con los requisitos de calidad y robustez. 

## Duración aproximada
- 30 minutos.

## Instrucciones

1. Métodos públicos.

- Los métodos públicos son accesibles desde fuera de la clase, lo que los convierte en los candidatos ideales para las pruebas unitarias. Se pueden invocar directamente en las pruebas y verificar sus resultados.

- *Ejemplo de método público*

- Supongamos que tenemos un componente llamado Calculadora con un método público llamado sumar.

```typescript

export class Calculadora {
  sumar(a: number, b: number): number {
    return a + b;
  }
}
```
- *Prueba de un método público*

- Para probar el método sumar, podemos escribir un test unitario de la siguiente manera:

```typescript

import { Calculadora } from './calculadora';

describe('Calculadora', () => {
  let calculadora: Calculadora;

  beforeEach(() => {
    calculadora = new Calculadora();
  });

  it('debería sumar dos números correctamente', () => {
    const resultado = calculadora.sumar(2, 3);
    expect(resultado).toBe(5);
  });
});

```

2. Métodos privados.

- Los métodos privados no son accesibles directamente desde fuera de la clase. Sin embargo, se pueden probar indirectamente al invocar métodos públicos que dependen de ellos. En general, es recomendable que las pruebas se centren en el comportamiento observable y no en la implementación interna.

- *Ejemplo de método privado*

- Supongamos que el componente Calculadora tiene un método privado llamado validarNumeros.

```typescript

export class Calculadora {
  public sumar(a: number, b: number): number {
    if (!this.validarNumeros(a, b)) {
      throw new Error('Números no válidos');
    }
    return a + b;
  }

  private validarNumeros(a: number, b: number): boolean {
    return typeof a === 'number' && typeof b === 'number';
  }
}
```

- *Prueba de un método privado*

- En este caso, no se probará directamente el método validarNumeros. En cambio, se puede verificar su efecto al llamar al método público sumar.

```typescript

describe('Calculadora', () => {
  let calculadora: Calculadora;

  beforeEach(() => {
    calculadora = new Calculadora();
  });

  it('debería lanzar un error si los números no son válidos', () => {
    expect(() => calculadora.sumar('a', 3)).toThrowError('Números no válidos');
  });
});

```

3. Métodos sin valor de retorno.

- Los métodos sin valor de retorno (void) generalmente realizan alguna acción o efecto colateral, como actualizar el estado del componente o realizar una operación externa. Se pueden probar verificando el estado del componente o el efecto esperado después de la invocación.

- *Ejemplo de método sin retorno*

```typescript

export class Contador {
  private cuenta: number = 0;

  public incrementar(): void {
    this.cuenta++;
  }

  public obtenerCuenta(): number {
    return this.cuenta;
  }
}
```

- *Prueba de un método sin retorno*

```typescript

describe('Contador', () => {
  let contador: Contador;

  beforeEach(() => {
    contador = new Contador();
  });

  it('debería incrementar la cuenta', () => {
    contador.incrementar();
    expect(contador.obtenerCuenta()).toBe(1);
  });
});
```

4. Buenas prácticas para probar métodos.

- Probar indirectamente métodos privados: En lugar de intentar acceder a métodos privados directamente, verificar su funcionalidad a través de métodos públicos que los utilicen.

- Centrarse en el comportamiento: Es más útil probar el comportamiento observable de los métodos en lugar de su implementación interna.

- Utilizar espías y mocks: Para métodos que dependen de otros servicios o funciones externas, considerar el uso de espías y mocks para controlar el entorno de prueba.


## Test de suscripciones (subscribe-observable) en Angular

1. ¿Qué son los Observables?

- Los Observables son una forma de representar flujos de datos que pueden emitir múltiples valores a lo largo del tiempo. A diferencia de las Promesas, que solo manejan un único valor, los Observables pueden enviar un número indefinido de valores y se pueden cancelar.

- *Ejemplo de un Observable*

```typescript

import { Observable } from 'rxjs';

const miObservable = new Observable(observer => {
  observer.next('Primer valor');
  observer.next('Segundo valor');
  observer.complete();
});
```

2. La Función subscribe.

- La función subscribe permite a los componentes recibir notificaciones de un Observable. Al subscribirse, se pueden definir funciones que manejen los valores emitidos, los errores y la finalización del flujo.

- *Ejemplo de uso de subscribe*

```typescript

miObservable.subscribe(
  valor => console.log(valor), // Manejar valores emitidos
  error => console.error(error), // Manejar errores
  () => console.log('Completo') // Manejar finalización
);
```

3. Test de suscripciones en componentes.

- Al realizar pruebas unitarias en componentes de Angular que utilizan Observables, es esencial verificar que las suscripciones manejen correctamente los datos emitidos. Generalmente, esto implica usar TestBed para configurar el entorno de prueba y fakeAsync o async para manejar la asincronía.

- *Ejemplo de Componente con Observable*

- Supongamos que hay un servicio que devuelve un Observable y un componente que se suscribe a él.

```typescript

import { Component, OnInit } from '@angular/core';
import { MiServicio } from './mi-servicio.service';

@Component({
  selector: 'app-mi-componente',
  template: `<div>{{ dato }}</div>`
})
export class MiComponente implements OnInit {
  dato: string;

  constructor(private miServicio: MiServicio) {}

  ngOnInit() {
    this.miServicio.obtenerDatos().subscribe(valor => {
      this.dato = valor;
    });
  }
}
```

4. Prueba del componente con suscripción.

- Para probar el comportamiento de este componente al recibir datos del servicio, configuramos el entorno de prueba utilizando TestBed.

- *Ejemplo de prueba*

```typescript

import { ComponentFixture, TestBed } from '@angular/core/testing';
import { MiComponente } from './mi-componente.component';
import { MiServicio } from './mi-servicio.service';
import { of } from 'rxjs';

describe('MiComponente', () => {
  let component: MiComponente;
  let fixture: ComponentFixture<MiComponente>;
  let miServicio: jasmine.SpyObj<MiServicio>;

  beforeEach(async () => {
    const servicioSpy = jasmine.createSpyObj('MiServicio', ['obtenerDatos']);
    
    await TestBed.configureTestingModule({
      declarations: [MiComponente],
      providers: [{ provide: MiServicio, useValue: servicioSpy }]
    }).compileComponents();

    miServicio = TestBed.inject(MiServicio) as jasmine.SpyObj<MiServicio>;
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(MiComponente);
    component = fixture.componentInstance;
  });

  it('debería asignar el dato al recibir un valor del servicio', () => {
    miServicio.obtenerDatos.and.returnValue(of('Valor de prueba'));
    
    component.ngOnInit(); // Llamar a ngOnInit para iniciar la suscripción
    fixture.detectChanges(); // Detectar cambios

    expect(component.dato).toBe('Valor de prueba');
  });
});

```

5. Buenas prácticas para probar suscripciones.

- Usar Spies y Mocks: Utilizar espías para simular el comportamiento de los servicios y controlar los datos que emiten.

- Manejar la Asincronía: Utilizar async o fakeAsync para manejar correctamente las operaciones asincrónicas en las pruebas.

- Verificar efectos colaterales: Asegurarse de comprobar que los efectos colaterales de las suscripciones (como la actualización de propiedades) se realicen como se espera.


## Test de Pipes en Angular

1. ¿Qué son los Pipes?

- Los Pipes son funciones que se utilizan para transformar datos en las plantillas de Angular. Por ejemplo, un Pipe puede formatear una fecha, convertir texto a mayúsculas, o filtrar una lista.

- *Ejemplo de un Pipe personalizado*

```typescript

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'capitalizar'
})
export class CapitalizarPipe implements PipeTransform {
  transform(value: string): string {
    return value ? value.charAt(0).toUpperCase() + value.slice(1) : '';
  }
}
```

En este ejemplo, el Pipe CapitalizarPipe transforma la primera letra de un texto a mayúscula.

2. Pruebas de Pipes.

- Las pruebas de pipes son simples, ya que se centran en verificar que el método transform del pipe devuelva el resultado esperado para diferentes entradas. Al ser funciones puras, los pipes son fáciles de testear.

- *Ejemplo de prueba para un Pipe*

- Para probar el Pipe CapitalizarPipe, se pueden escribir pruebas unitarias que verifiquen su comportamiento con diferentes cadenas de texto.

```typescript

import { CapitalizarPipe } from './capitalizar.pipe';

describe('CapitalizarPipe', () => {
  let pipe: CapitalizarPipe;

  beforeEach(() => {
    pipe = new CapitalizarPipe();
  });

  it('debería capitalizar la primera letra de una cadena', () => {
    expect(pipe.transform('hola')).toBe('Hola');
  });

  it('debería retornar una cadena vacía si el valor es vacío', () => {
    expect(pipe.transform('')).toBe('');
  });

  it('debería manejar valores nulos', () => {
    expect(pipe.transform(null)).toBe('');
  });

  it('debería capitalizar solo la primera letra, dejando el resto igual', () => {
    expect(pipe.transform('hOLA')).toBe('HOLA');
  });
});
```

3. Buenas prácticas al probar Pipes.

*Verificar diferentes casos de entrada*: Asegurarse de probar una variedad de entradas, incluyendo cadenas vacías, nulas y valores especiales.

*Mantener las pruebas simples*: Dado que los Pipes son funciones puras, las pruebas deben ser simples y centrarse en la entrada y salida del método transform.

*Utilizar descripciones claras*: Usar descripciones claras en las pruebas para que sea evidente lo que se está validando en cada caso.

4. Ejecución de pruebas.

- Para ejecutar las pruebas de los Pipes, se puede utilizar el comando habitual de Angular:

```bash

ng test
```

- Esto iniciará el framework de pruebas configurado (generalmente Karma y Jasmine) y ejecutará todas las pruebas, incluyendo las de los Pipes.


## Test de servicios (con Peticiones a API) en Angular

1. ¿Qué es un servicio en Angular?

- Un servicio en Angular es una clase que se utiliza para encapsular y compartir funcionalidades a lo largo de la aplicación. Esto incluye la gestión de datos, la lógica de negocio y la comunicación con APIs externas. Los servicios se suelen inyectar en componentes o en otros servicios mediante la inyección de dependencias.

- *Ejemplo de un servicio*

```typescript

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class ApiService {
  private apiUrl = 'https://api.example.com/datos';

  constructor(private http: HttpClient) {}

  obtenerDatos(): Observable<any> {
    return this.http.get(this.apiUrl);
  }
}
```

2. Pruebas de servicios con peticiones a API.

- Al realizar pruebas en servicios que utilizan HttpClient para realizar peticiones a APIs, se recomienda utilizar HttpClientTestingModule, que proporciona herramientas para simular las solicitudes HTTP y controlar las respuestas.

- *Configuración del entorno de pruebas*

- Antes de escribir las pruebas, es necesario importar HttpClientTestingModule y HttpTestingController.

```typescript

import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { ApiService } from './api.service';

describe('ApiService', () => {
  let service: ApiService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [ApiService],
    });

    service = TestBed.inject(ApiService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify(); // Verifica que no haya solicitudes HTTP pendientes
  });
});
```

3. Ejemplo de pruebas

- A continuación, se muestra cómo se pueden realizar pruebas para el método obtenerDatos del servicio ApiService.

- *Prueba de éxito en la petición*

```typescript

it('debería retornar datos al realizar una petición exitosa', () => {
  const mockDatos = [{ id: 1, nombre: 'Dato 1' }, { id: 2, nombre: 'Dato 2' }];

  service.obtenerDatos().subscribe(datos => {
    expect(datos).toEqual(mockDatos);
  });

  const req = httpMock.expectOne('https://api.example.com/datos');
  expect(req.request.method).toBe('GET'); // Verificar el método de la solicitud
  req.flush(mockDatos); // Simular la respuesta de la API
});
```

- *Prueba de error en la petición*

```typescript

it('debería manejar errores al realizar la petición', () => {
  const errorMsg = 'Error de red';

  service.obtenerDatos().subscribe(
    () => fail('Se esperaba un error, no datos'),
    error => {
      expect(error.status).toBe(500);
      expect(error.error).toContain(errorMsg);
    }
  );

  const req = httpMock.expectOne('https://api.example.com/datos');
  req.flush(errorMsg, { status: 500, statusText: 'Error de red' }); // Simular un error
});
```

4. Buenas prácticas al probar servicios.

*Utilizar HttpClientTestingModule*: Esto permite realizar pruebas sin necesidad de realizar llamadas reales a la API.

*Verificar que no queden solicitudes pendientes*: Siempre verifica que no haya solicitudes HTTP no respondidas utilizando httpMock.verify().

*Probar casos positivos y negativos*: Asegurarse de probar tanto las respuestas exitosas como los escenarios de error para tener una cobertura completa.

*Utilizar mocks y spies*: Para servicios que dependen de otros servicios, utiliza mocks y spies para aislar las pruebas y verificar el comportamiento de los servicios sin depender de su implementación.


## Mock de servicios en Angular

1. ¿Qué es un Servicio en Angular?

Un servicio en Angular es una clase que contiene lógica de negocio y funcionalidades que pueden ser compartidas entre diferentes componentes de la aplicación. Los servicios son una parte esencial de la arquitectura de Angular, ya que permiten organizar y encapsular el código de manera eficiente.

- *Ejemplo de un servicio*

```typescript

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class ApiService {
  constructor(private http: HttpClient) {}

  obtenerDatos(): Observable<any> {
    return this.http.get('https://api.example.com/datos');
  }
}
```

2. ¿Por qué usar Mocks?

-  Los mocks son útiles en pruebas unitarias por varias razones:

- *Aislamiento*: Permiten probar componentes o servicios de manera aislada, sin interferencias de otros componentes o servicios.

- *Control*: Se puede controlar el comportamiento del servicio simulado, como devolver valores específicos o lanzar errores.

- *Eficiencia*: Al no realizar llamadas a APIs externas, las pruebas son más rápidas y no dependen de la disponibilidad del servidor.

- *Flexibilidad*: Se pueden simular diferentes escenarios de respuesta sin modificar el código real.



3. Cómo crear un mock de servicio.

-Existen diferentes maneras de crear un mock de servicio en Angular. Una de las más comunes es usar clases simuladas que implementen la misma interfaz que el servicio real.

-*Ejemplo de un mock de servicio*

```typescript

class MockApiService {
  obtenerDatos() {
    return of([{ id: 1, nombre: 'Dato 1' }, { id: 2, nombre: 'Dato 2' }]);
  }
}
```

4. Usar el mock en pruebas.

- Para utilizar el mock en pruebas unitarias, se inyecta la clase simulada en lugar del servicio real utilizando el TestBed.

-  *Ejemplo de prueba con mock*

```typescript

import { TestBed } from '@angular/core/testing';
import { MiComponente } from './mi-componente.component';
import { ApiService } from './api.service';
import { MockApiService } from './mock-api.service'; // Importar el mock

describe('MiComponente', () => {
  let component: MiComponente;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [MiComponente],
      providers: [{ provide: ApiService, useClass: MockApiService }] // Usar el mock
    });

    const fixture = TestBed.createComponent(MiComponente);
    component = fixture.componentInstance;
  });

  it('debería obtener datos al inicializar', () => {
    component.ngOnInit();
    expect(component.datos.length).toBe(2);
    expect(component.datos[0].nombre).toBe('Dato 1');
  });
});
```

5. Buenas prácticas al usar mocks.

*Aislar pruebas*: Siempre que sea posible, utilizar mocks para aislar las pruebas de los componentes o servicios de su lógica subyacente.

*Simular respuestas realistas*: Asegurarse de que el comportamiento del mock sea lo más similar posible al servicio real para que las pruebas sean significativas.

*Probar diferentes escenarios*: Crear diferentes mocks o configura el mismo mock para simular distintos escenarios, como respuestas exitosas o errores.

*Limitar dependencias*: Reducir la complejidad de las pruebas limitando las dependencias a lo estrictamente necesario.


