## Crear el componente para seleccionar y subir una imagen

### HTML del componente

Creamos un botón para examinar las imágenes del equipo y otro para guardar la imagen seleccionada.

```html
<div class="container mt-5 text-center">
    <h2 class="mb-4">Subir imagen</h2>

    <label for="imagen" class="btn btn-primary">
        Examinar imagen
    </label>

    <input
        #imagenInput
        type="file"
        id="imagen"
        accept="image/*"
        class="d-none"
    />

    <br>
    <br>

    <button
        class="btn btn-success"
        (click)="seleccionarImagen()">
        Guardar imagen seleccionada
    </button>
</div>
```

**Elementos importantes:**

- `type="file"`: permite seleccionar un fichero del equipo.
- `accept="image/*"`: limita la selección a imágenes.
- `#imagenInput`: referencia al input para poder acceder a él desde TypeScript.
- `class="d-none"`: oculta el input.
- `(click)="seleccionarImagen()"`: ejecuta el método al pulsar el botón.

### TypeScript del componente

```typescript
import { Component, ElementRef, ViewChild } from '@angular/core';
import { Service } from '../../services/service';

@Component({
  selector: 'app-subir-component',
  standalone: false,
  templateUrl: './subir-component.html',
  styleUrl: './subir-component.css',
})
export class SubirComponent {

  @ViewChild('imagenInput') imagenInput!: ElementRef;

  public imagen!: File;

  constructor(
    private _service: Service
  ) {}

  async seleccionarImagen(): Promise<void> {

    const seleccionada = this.imagenInput.nativeElement;

    if (seleccionada.files) {
      this.imagen = seleccionada.files[0];

      console.log(this.imagen);

      await this._service.subirFoto(this.imagen);
    }
  }
}
```

### Explicación del código

**1. ViewChild**

```typescript
@ViewChild('imagenInput') imagenInput!: ElementRef;
```

Utilizamos `ViewChild` para acceder al input del HTML mediante la referencia `#imagenInput`.

**2. Variable para almacenar la imagen**

```typescript
public imagen!: File;
```

Guardamos el fichero seleccionado en una variable de tipo `File`.

**3. Acceder al input**

```typescript
const seleccionada = this.imagenInput.nativeElement;
```

Accedemos al elemento HTML desde TypeScript.

**4. Comprobar si hay ficheros seleccionados**

```typescript
if (seleccionada.files) {
```

Comprobamos que el input tenga disponible la propiedad `files`.

**5. Obtener la imagen**

```typescript
this.imagen = seleccionada.files[0];
```

Obtenemos el primer fichero seleccionado y lo guardamos en `imagen`.

**6. Enviar la imagen al servicio**

```typescript
await this._service.subirFoto(this.imagen);
```

Llamamos al método `subirFoto()` del servicio, que convierte la imagen a Base64 y la envía al servidor.

### Funcionamiento completo

1. Pulsamos **Examinar imagen**.
2. Seleccionamos una imagen del equipo.
3. Pulsamos **Guardar imagen seleccionada**.
4. El componente obtiene el fichero mediante `ViewChild`.
5. Se envía el fichero al servicio `subirFoto()`.
6. El servicio convierte la imagen a Base64 y realiza la petición HTTP POST.
