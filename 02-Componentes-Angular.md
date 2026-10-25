
# COMPONENTES EN ANGULAR

## 1. Estructura de un componente

Un componente en Angular se compone de:

- **TypeScript (`.ts`):** contiene la funcionalidad y las variables.
- **HTML (`.html`):** contiene la vista.
- **CSS (`.css`):** contiene los estilos.

No es obligatorio tener archivos HTML y CSS separados.

## 2. Crear un componente

Ejemplo de un componente llamado `PrimerComponent`:

```typescript
import { Component } from "@angular/core";

// SIEMPRE DEBEMOS DECLARAR
@Component({
    // NOMBRE QUE SE USARÁ EN LAS VISTAS
    selector: "primer-component",
    standalone: false,

    // NO NECESITAMOS NI HTML NI CSS OBLIGATORIAMENTE

    // HTML
    templateUrl: './primer.component.html',

    // CSS
    styleUrls: ["./primer.component.css"]
})

// SIEMPRE DEBE TENER UNA CLASE
export class PrimerComponent {
    public nombre: string;
    public sports: Array<string>;
    public year: number;

    constructor() {
        this.nombre = "Iker";
        this.year = 2026;
        this.numbers = [2, 67, 90, 345, 35];
    }
}
```

## 3. Mostrar variables en HTML

Para mostrar en la vista las variables declaradas en el componente, utilizamos `{{ }}`.

Ejemplo en el archivo HTML:

```html
<h1>Holaaa</h1>
<h3>{{ nombre }}</h3>
<h3>{{ year }}</h3>
```

## 4. Estilos del componente

Podemos indicar los estilos del componente de dos maneras:

- `styleUrls`: permite indicar un array de archivos CSS.
- `styleUrl`: permite indicar un único archivo CSS.

## 5. Declarar el componente en app-module

Para poder utilizar el componente, debemos importarlo y declararlo en el módulo principal.

Importamos el componente:

```typescript
import { PrimerComponent } from '../components/primercomponent/primer.component';
```

Después, lo añadimos a `declarations`:

```typescript
@NgModule({
    declarations: [
        App,
        PrimerComponent
    ],
})
```

## 6. Utilizar el componente en HTML

Una vez declarado, podemos utilizarlo en el archivo `app.html` mediante el selector definido en el componente:

```html
<primer-component></primer-component>
```
