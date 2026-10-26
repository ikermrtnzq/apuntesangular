# 09. Lectura de archivos JSON en Angular

## 1. Leer un JSON

Para leer archivos del servidor necesitamos utilizar acciones asíncronas. Los archivos deben estar dentro de la carpeta `public/assets`, según la estructura de nuestro proyecto.

Para leer archivos podemos utilizar `fetch()`, que permite obtener archivos JSON, binarios o de texto plano.

Es recomendable convertir los datos del fichero en objetos del modelo (`Model`) para poder trabajar con ellos desde el componente.

## 2. Cargar los datos con `fetch()`

Por ejemplo, para leer un archivo `comics.json` y convertir sus datos en objetos `Comic`:

```typescript
cargarComics(): void {
  let path = "./assets/comics.json";

  fetch(path)
    .then(response => response.json())
    .then((data: any[]) => {
      for (let elem of data) {
        let comic = new Comic(
          elem.nombre,
          elem.imagen,
          elem.descripcion
        );

        this.comics.push(comic);
      }

      this._cdr.detectChanges();
    });
}
```

### ¿Qué hace el código?

1. **`fetch(path)`**: solicita el archivo JSON.
2. **`response.json()`**: convierte la respuesta en datos JavaScript.
3. **`data`**: contiene los elementos leídos del archivo.
4. **`for...of`**: recorre los elementos del JSON.
5. **`new Comic(...)`**: crea un objeto `Comic` para cada elemento.
6. **`this.comics.push(comic)`**: añade cada objeto al array de cómics.
7. **`this._cdr.detectChanges()`**: solicita a Angular que actualice la vista.

## 3. Inyectar `ChangeDetectorRef`

Como `fetch()` es asíncrono, en estos apuntes se utiliza `ChangeDetectorRef` para actualizar la vista cuando se reciben los datos.

Se importa desde Angular y se inyecta en el constructor:

```typescript
import { ChangeDetectorRef } from "@angular/core";

constructor(private _cdr: ChangeDetectorRef) {}
```

Después se puede utilizar:

```typescript
this._cdr.detectChanges();
```

## 4. Mostrar los datos en el HTML

Una vez cargados los cómics en el array `comics`, se pueden mostrar recorriendo la colección con `*ngFor`:

```html
<div *ngFor="let comic of comics">
  <h3>{{ comic.nombre }}</h3>
  <img [src]="comic.imagen" [alt]="comic.nombre">
  <p>{{ comic.descripcion }}</p>
</div>
```

El HTML recorre el array y muestra los datos de cada objeto `Comic`.

## 5. Resumen

- `fetch()` permite leer un archivo de forma asíncrona.
- `response.json()` obtiene los datos del JSON.
- Es recomendable transformar los datos en objetos del modelo, como `Comic`.
- Los objetos se almacenan en un array para mostrarlos en el componente.
- `ChangeDetectorRef` y `detectChanges()` se utilizan en este ejemplo para actualizar la vista.

**Nota:** la ruta `./assets/comics.json` es la que aparece en el ejemplo. Debe coincidir con la ubicación real del archivo en el proyecto.
