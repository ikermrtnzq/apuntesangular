# 10. Lectura de archivos Excel en Angular

## 1. Instalar la librería `xlsx`

Para leer archivos Excel desde Angular utilizamos la librería `xlsx`.

La instalamos con el siguiente comando:

```bash
npm install xlsx --save
```

Después, importamos la librería en el componente:

```typescript
import * as XLSX from 'xlsx';
```

## 2. Leer el archivo Excel

Para leer un archivo Excel utilizamos `fetch()` y obtenemos su contenido como un `ArrayBuffer`.

```typescript
cargarComics(): void {
  let path = "./assets/comics.xlsx";

  fetch(path)
    .then(response => response.arrayBuffer())
    .then(buffer => {
      let workbook = XLSX.read(buffer, { type: "array" });

      let hojaName = workbook.SheetNames[0];
      let hoja = workbook.Sheets[hojaName];

      let data = XLSX.utils.sheet_to_json(hoja);

      this.comics = data.map((c: any) =>
        new Comic(c.nombre, c.imagen, c.descripcion)
      );

      this._cdr.detectChanges();
    });
}
```

## 3. ¿Qué hace el código?

1. **`fetch(path)`**: solicita el archivo Excel.
2. **`response.arrayBuffer()`**: obtiene el contenido del archivo como datos binarios.
3. **`XLSX.read(buffer, { type: "array" })`**: lee el archivo y crea un objeto `workbook`.
4. **`workbook.SheetNames[0]`**: obtiene el nombre de la primera hoja del Excel.
5. **`workbook.Sheets[hojaName]`**: obtiene esa hoja.
6. **`XLSX.utils.sheet_to_json(hoja)`**: convierte las filas de la hoja en datos JavaScript.
7. **`data.map(...)`**: transforma cada fila en un objeto `Comic`.
8. **`this._cdr.detectChanges()`**: solicita la actualización de la vista.

## 4. Mostrar los datos en el HTML

Una vez que los datos están almacenados en `this.comics`, podemos recorrer el array en la plantilla:

```html
<div *ngFor="let comic of comics">
  <h3>{{ comic.nombre }}</h3>
  <img [src]="comic.imagen" [alt]="comic.nombre">
  <p>{{ comic.descripcion }}</p>
</div>
```

El HTML es básicamente igual que cuando leemos los datos desde un JSON, porque en ambos casos acabamos trabajando con objetos `Comic`.

## 5. Resumen

- Instalamos la librería con `npm install xlsx --save`.
- Importamos la librería mediante `import * as XLSX from 'xlsx'`.
- Utilizamos `response.arrayBuffer()` para obtener el contenido del Excel.
- `XLSX.read()` lee el archivo.
- `XLSX.utils.sheet_to_json()` convierte una hoja en datos JavaScript.
- Transformamos los datos en objetos del modelo `Comic`.
- Mostramos los objetos en el HTML recorriendo el array `comics`.
