# 11. Servicios en Angular

## 1. ¿Qué es un servicio?

Un servicio en Angular es una clase que contiene métodos para realizar tareas, como obtener datos de un archivo JSON.

En lugar de escribir toda la lógica para cargar los datos dentro del componente, podemos trasladarla a un servicio y hacer que el componente lo utilice.

Esto permite separar la lógica de obtención de datos de la lógica de presentación.

## 2. Crear un servicio

Para crear un servicio que obtenga los cómics desde un archivo JSON, utilizamos `@Injectable()`.

```typescript
import { Injectable } from "@angular/core";
import { Comic } from "../models/comic";

@Injectable()
export class ServiceComics {

  getComics(): Promise<Array<Comic>> {
    let path = "./assets/comics.json";

    return fetch(path)
      .then(response => response.json())
      .then((data: any[]) => {
        let comics: Array<Comic> = [];

        for (let elem of data) {
          let comic = new Comic(
            elem.nombre,
            elem.imagen,
            elem.descripcion
          );

          comics.push(comic);
        }

        return comics;
      });
  }
}
```

### Explicación

- **`@Injectable()`**: permite que la clase participe en el sistema de inyección de dependencias de Angular.
- **`ServiceComics`**: es la clase del servicio.
- **`getComics()`**: obtiene los datos y devuelve una `Promise` que contiene un array de objetos `Comic`.
- **`return fetch(...)`**: devuelve la promesa encadenada para que el componente pueda esperar los resultados.
- **`return comics`**: devuelve el array con todos los cómics.

## 3. Registrar el servicio en `app.module.ts`

Para que Angular pueda proporcionar el servicio, lo añadimos al array `providers` del módulo:

```typescript
providers: [
  provideBrowserGlobalErrorListeners(),
  ServiceComics
]
```

Así, Angular podrá inyectar `ServiceComics` en los componentes que lo necesiten.

## 4. Utilizar el servicio desde un componente

Importamos el servicio y lo inyectamos en el constructor junto con `ChangeDetectorRef`.

```typescript
import { ChangeDetectorRef, Component, OnInit } from "@angular/core";
import { Comic } from "../models/comic";
import { ServiceComics } from "../services/service-comics";

export class ComicsComponent implements OnInit {

  public comics: Array<Comic> = [];

  constructor(
    private _cdr: ChangeDetectorRef,
    private _service: ServiceComics
  ) {}

  ngOnInit(): void {
    this._service.getComics().then(data => {
      this.comics = data;
      this._cdr.detectChanges();
    });
  }
}
```

### ¿Qué ocurre?

1. Angular crea el componente.
2. Se ejecuta `ngOnInit()`.
3. El componente llama al método `getComics()` del servicio.
4. Cuando termina la operación asíncrona, el servicio devuelve los datos.
5. El componente guarda los datos en `this.comics`.
6. Se llama a `detectChanges()` para solicitar la actualización de la vista.

## 5. Utilizar `async` y `await`

También podemos escribir el método del servicio utilizando `async` y `await`, en lugar de encadenar varios `.then()`.

```typescript
async getComicsAsync(): Promise<Array<Comic>> {
  let path = "./assets/comics.json";

  let response = await fetch(path);
  let data = await response.json();

  let comics: Array<Comic> = [];

  for (let elem of data) {
    let comic = new Comic(
      elem.nombre,
      elem.imagen,
      elem.descripcion
    );

    comics.push(comic);
  }

  return comics;
}
```

- **`async`**: indica que el método trabaja de forma asíncrona y devuelve una promesa.
- **`await`**: espera a que se resuelva una promesa antes de continuar.

En el componente podemos utilizarlo así:

```typescript
async ngOnInit(): Promise<void> {
  this.comics = await this._service.getComicsAsync();
  this._cdr.detectChanges();
}
```

## 6. Mostrar los datos en el HTML

El HTML sigue siendo igual, porque el componente continúa teniendo un array de objetos `Comic`.

```html
<div *ngFor="let comic of comics">
  <h3>{{ comic.nombre }}</h3>
  <img [src]="comic.imagen" [alt]="comic.nombre">
  <p>{{ comic.descripcion }}</p>
</div>
```

## 7. Resumen

- Los servicios permiten separar la obtención de datos de los componentes.
- `@Injectable()` identifica la clase como utilizable mediante inyección de dependencias.
- `providers` permite registrar el servicio.
- El servicio se inyecta en el constructor del componente.
- `Promise<Array<Comic>>` indica que el método devuelve una promesa con un array de cómics
