# 12. Servicios con HttpClient en Angular

## 1. ¿Qué es HttpClient?

`HttpClient` es una herramienta de Angular que permite realizar peticiones HTTP a servicios y API para obtener datos.

En lugar de utilizar `fetch()` directamente, podemos utilizar `HttpClient` dentro de un servicio.

Los métodos de `HttpClient`, como `get()`, devuelven un `Observable<Type>`, que permite recibir datos de forma asíncrona.

## 2. Configurar HttpClient

Para utilizar `HttpClient`, importamos `provideHttpClient` desde Angular:

```typescript
import { provideHttpClient } from "@angular/common/http";
```

Después, lo añadimos al array `providers` de la configuración de la aplicación:

```typescript
providers: [
  provideBrowserGlobalErrorListeners(),
  provideHttpClient()
]
```

Una vez configurado, podemos inyectar `HttpClient` en los servicios que lo necesiten.

## 3. Crear el servicio

Creamos el servicio `ServicePersonas`, que se encargará de realizar la petición a la API.

```typescript
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Observable } from "rxjs";

@Injectable()
export class ServicePersonas {
  constructor(private _http: HttpClient) {}

  getPersonas(): Observable<any> {
    let url =
      "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas";

    return this._http.get(url);
  }
}
```

### Explicación

- **`HttpClient`**: permite realizar peticiones HTTP.
- **`constructor(private _http: HttpClient)`**: inyecta `HttpClient` en el servicio.
- **`Observable<any>`**: indica que el método devuelve un observable.
- **`getPersonas()`**: devuelve el resultado de la petición.
- **`this._http.get(url)`**: realiza una petición GET a la URL indicada.

## 4. Registrar el servicio en providers

Además de configurar `provideHttpClient()`, los apuntes indican que hay que añadir `ServicePersonas` a `providers`:

```typescript
providers: [
  provideBrowserGlobalErrorListeners(),
  provideHttpClient(),
  ServicePersonas
]
```

Así, Angular puede proporcionar el servicio al componente que lo necesite.

## 5. Utilizar el servicio desde el componente con subscribe()

El componente inyecta `ServicePersonas` y se suscribe al observable para recibir los datos.

```typescript
import {
  ChangeDetectorRef,
  Component,
  OnInit
} from "@angular/core";
import { Persona } from "../models/persona";
import { ServicePersonas } from "../services/service-personas";

export class PersonasApiComponent implements OnInit {
  public personas!: Array<Persona>;

  constructor(
    private _cdr: ChangeDetectorRef,
    private _service: ServicePersonas
  ) {}

  ngOnInit(): void {
    this._service.getPersonas().subscribe({
      next: (response) => {
        this.personas = response;
        this._cdr.detectChanges();
      },
      error: (error) => {
        console.log(error);
      }
    });
  }
}
```

### ¿Qué ocurre?

1. Se ejecuta `ngOnInit()` cuando se inicializa el componente.
2. Se llama al método `getPersonas()` del servicio.
3. `subscribe()` permite recibir la respuesta del observable.
4. En `next`, los datos se guardan en `this.personas`.
5. En `error`, se muestra el error en la consola.
6. Se llama a `detectChanges()` para solicitar la actualización de la vista.

## 6. Mostrar los datos en el HTML

Una vez recibidos los datos, podemos recorrer el array de personas con `*ngFor`.

```html
<div *ngFor="let persona of personas">
  <p>{{ persona.nombre }}</p>
</div>
```

El contenido que mostremos dependerá de las propiedades del modelo `Persona` y de los datos devueltos por la API.

## 7. Convertir el Observable en una Promise

Los apuntes también muestran cómo crear un método que devuelva una `Promise` en lugar de devolver directamente un `Observable`.

```typescript
getPersonasPromesa(): Promise<any> {
  let url =
    "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas";

  let promise = new Promise((resolve) => {
    this._http.get(url).subscribe((response) => {
      resolve(response);
    });
  });

  return promise;
}
```

### Llamar al método desde el componente

Podemos utilizar `.then()` para recibir los datos y `.catch()` para gestionar errores:

```typescript
ngOnInit(): void {
  this._service.getPersonasPromesa()
    .then((response) => {
      this.personas = response;
      this._cdr.detectChanges();
    })
    .catch((error: any) => {
      console.log(error);
    });
}
```

**Idea principal:** el servicio transforma la respuesta del observable en una promesa, que después se consume con `.then()` y `.catch()`.

> En el documento original, el nombre de la llamada aparece escrito como `getPersonasPromsa()`, mientras que el método se declara como `getPersonasPromesa()`. Ambos nombres deben coincidir.

## 8. Usar firstValueFrom() con async/await

Otra alternativa de los apuntes es convertir el observable en una promesa utilizando `firstValueFrom()` y trabajar con `async/await`.

Importamos la función desde RxJS:

```typescript
import { firstValueFrom } from "rxjs";
```

### Método del servicio

```typescript
async getPersonasAsync(): Promise<Persona[]> {
  let url =
    "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas";

  return firstValueFrom(
    this._http.get<Persona[]>(url)
  );
}
```

En este caso, `this._http.get<Persona[]>(url)` especifica que la respuesta esperada es un array de `Persona`.

### Llamar al método desde el componente

```typescript
async ngOnInit(): Promise<void> {
  try {
    this.personas =
      await this._service.getPersonasAsync();

    this._cdr.detectChanges();
  } catch (error) {
    console.log(error);
  }
}
```

### ¿Qué ocurre?

1. `async` permite utilizar `await` dentro del método.
2. `await` espera a que se resuelva la promesa devuelta por el servicio.
3. Los datos se guardan en `this.personas`.
4. `detectChanges()` solicita la actualización de la vista.
5. `try/catch` permite gestionar los errores.

## 9. Diferencias entre fetch y HttpClient

| Característica | `fetch()` | `HttpClient` |
|---|---|---|
| Forma de trabajar | Devuelve una `Promise` | Devuelve un `Observable` |
| Recibir el resultado | `.then()` o `await` | `.subscribe()` |
| Conversión para usar promesas | No es necesaria para usar `await` | Se puede usar `firstValueFrom()` |
| Uso en estos apuntes | Leer archivos JSON | Consultar una API desde un servicio |

## 10. Resumen

- `provideHttpClient()` configura `HttpClient` en la aplicación.
- `ServicePersonas` centraliza las peticiones a la API.
- `HttpClient` se inyecta en el servicio mediante el constructor.
- `get(url)` realiza una petición GET y devuelve un `Observable`.
- `subscribe()` permite recibir los datos y gestionar la respuesta con `next` y `error`.
- Podemos mostrar los datos en el HTML usando `*ngFor`.
- También podemos convertir el observable en una `Promise` y usar `.then()` y `.catch()`.
- Con `firstValueFrom()` podemos usar `async/await` y gestionar errores con `try/catch`.
