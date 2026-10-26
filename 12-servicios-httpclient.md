# 12. Servicios con HttpClient en Angular

## 1. ¿Qué es HttpClient?

`HttpClient` es una herramienta de Angular que permite realizar peticiones HTTP a servicios y API para obtener datos.

En lugar de utilizar `fetch()` directamente, podemos utilizar `HttpClient` dentro de un servicio.

Una diferencia importante es que los métodos de `HttpClient`, como `get()`, devuelven un `Observable`.

## 2. Configurar HttpClient

Para poder utilizar `HttpClient`, debemos importar `provideHttpClient` desde Angular:

```typescript
import { provideHttpClient } from "@angular/common/http";
```

Después, lo añadimos al array `providers` de la configuración de la aplicación:

```typescript
providers: [
  provideBrowserGlobalErrorListeners(),
  provideHttpClient(),
  ServicePersonas
]
```

Así, Angular podrá proporcionar `HttpClient` a los servicios que lo necesiten.

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
    let url = "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas";

    return this._http.get(url);
  }
}
```

### Explicación

- **`HttpClient`**: permite realizar peticiones HTTP.
- **`constructor(private _http: HttpClient)`**: inyecta `HttpClient` en el servicio.
- **`Observable`**: representa un flujo de datos que puede recibirse de forma asíncrona.
- **`getPersonas()`**: devuelve el resultado de la petición.
- **`this._http.get(url)`**: realiza una petición GET a la URL indicada.

## 4. Utilizar el servicio desde el componente

El componente inyecta `ServicePersonas` y se suscribe al observable para recibir los datos.

```typescript
import { ChangeDetectorRef, Component, OnInit } from "@angular/core";
import { Persona } from "../models/persona";
import { ServicePersonas } from "../services/service-personas";

export class PersonasApiComponent implements OnInit {

  public personas!: Array<Persona>;

  constructor(
    private _cdr: ChangeDetectorRef,
    private _service: ServicePersonas
  ) {}

  ngOnInit(): void {
    this._service.getPersonas().subscribe((response) => {
      this.personas = response;
      this._cdr.detectChanges();
    });
  }
}
```

### ¿Qué ocurre?

1. Se ejecuta `ngOnInit()` cuando se inicializa el componente.
2. Se llama al método `getPersonas()` del servicio.
3. **`.subscribe()`** permite recibir la respuesta del observable.
4. Los datos recibidos se guardan en `this.personas`.
5. Se llama a `detectChanges()` para solicitar la actualización de la vista.

## 5. Mostrar los datos en el HTML

Una vez recibidos los datos, podemos recorrer el array de personas con `*ngFor`.

```html
<div *ngFor="let persona of personas">
  <p>{{ persona.nombre }}</p>
</div>
```

El contenido concreto que mostremos dependerá de las propiedades que tenga el modelo `Persona` y de los datos devueltos por la API.

## 6. Diferencias entre fetch y HttpClient

| Característica | `fetch()` | `HttpClient` |
|---|---|---|
| Forma de trabajar | Devuelve una `Promise` | Devuelve un `Observable` |
| Recibir el resultado | `.then()` o `await` | `.subscribe()` |
| Uso en estos apuntes | Leer archivos JSON | Consultar una API desde un servicio |

## 7. Resumen

- `provideHttpClient()` configura `HttpClient` en la aplicación.
- `HttpClient` se inyecta en el servicio mediante el constructor.
- `get(url)` realiza una petición GET y devuelve un `Observable`.
- `subscribe()` permite recibir los datos del observable.
- El componente guarda la respuesta y muestra los datos en el HTML.
