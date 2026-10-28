
# 17. CRUD API: GET, POST, UPDATE y DELETE

CRUD representa las cuatro operaciones principales que se realizan sobre los datos de una API:

- **GET:** obtener datos.
- **POST:** crear datos nuevos.
- **UPDATE (PUT):** modificar datos existentes.
- **DELETE:** eliminar datos.

En este ejemplo trabajamos con departamentos y utilizamos un servicio de Angular para realizar las peticiones HTTP a la API.

## 1. GET: obtener departamentos

En el servicio se crea un método asíncrono para obtener la lista de departamentos desde la API.

### Método en el servicio

```typescript
async getDepartamentos(): Promise<Departamento[]> {
  let endpoint = "api/departamentos";

  return firstValueFrom(
    this._http.get<Departamento[]>(this.url + endpoint)
  );
}
```

El método realiza una petición HTTP GET y devuelve una promesa con un array de objetos `Departamento`.

- `this._http.get<Departamento[]>()`: realiza la petición GET e indica que la respuesta es un array de departamentos.
- `firstValueFrom()`: convierte el Observable de Angular en una promesa.
- `Promise<Departamento[]>`: indica que el método devuelve una promesa que contiene un array de departamentos.

### Llamada desde el componente

En el componente se utiliza `OnInit` para cargar los departamentos al inicializarse.

```typescript
export class HomeComponent implements OnInit {
  public departamentos!: Array<Departamento>;

  constructor(
    private _service: ServiceDepartamentos,
    private _cdr: ChangeDetectorRef
  ) {}

  async ngOnInit(): Promise<void> {
    this.departamentos =
      await this._service.getDepartamentos();

    this._cdr.detectChanges();
  }
}
```

El componente llama al servicio, espera el resultado y lo asigna a la propiedad `departamentos`. Después, utiliza `detectChanges()` para actualizar la detección de cambios.

- `OnInit`: permite ejecutar código cuando se inicializa el componente.
- `async` y `await`: permiten esperar el resultado de una operación asíncrona.
- `ChangeDetectorRef`: permite controlar la detección de cambios.
- `detectChanges()`: comprueba los cambios y actualiza la vista.

---

## 2. POST: crear un departamento

Para crear un departamento, se envían sus datos a la API mediante una petición HTTP POST.

Es necesario enviar el objeto con su información y establecer la cabecera `Content-Type` para indicar que los datos se envían en formato JSON.

### Importar HttpHeaders

En el servicio se importa `HttpHeaders` para configurar la cabecera de la petición.

```typescript
import { HttpHeaders } from "@angular/common/http";
```

### Método en el servicio

```typescript
async createDepartamento(
  departamento: Departamento
): Promise<any> {
  // Convertimos el objeto a JSON
  let json = JSON.stringify(departamento);

  // Creamos un objeto header para el tipo de contenido
  let header = new HttpHeaders();

  // Incluimos la clave en el header
  header = header.set("content-type", "application/json");

  let endpoint = "api/departamentos";

  return firstValueFrom(
    this._http.post(
      this.url + endpoint,
      json,
      { headers: header }
    )
  );
}
```

Funcionamiento:

1. Recibe un objeto de tipo `Departamento`.
2. `JSON.stringify()` convierte el objeto en una cadena JSON.
3. Se crea una cabecera HTTP con `HttpHeaders`.
4. Se establece `content-type: application/json`.
5. Se realiza la petición POST enviando los datos.
6. `firstValueFrom()` convierte el Observable en una promesa.

### Formulario HTML

Creamos un formulario para introducir el identificador, el nombre y la localidad del departamento.

```html
<form #formCreate="ngForm" action=""
  style="display: flex; flex-direction: column; gap: 10px;">

  <label>Id Departamento:</label>
  <input name="cajaid" #cajaid type="number">

  <label>Nombre:</label>
  <input name="cajanombre" #cajanombre type="text">

  <label>Localidad:</label>
  <input name="cajalocalidad" #cajalocalidad type="text">

  <button (click)="insertDept()"
    style="background-color: lightblue; border: 1px solid lightblue; border-radius: 8px;">
    create
  </button>
</form>
```

Las referencias `#cajaid`, `#cajanombre` y `#cajalocalidad` permiten acceder a los campos desde el componente mediante `@ViewChild`.

### Componente CreateComponent

```typescript
export class CreateComponent {
  public mensaje!: string;

  @ViewChild("cajaid") cajaId!: ElementRef;
  @ViewChild("cajanombre") cajaNombre!: ElementRef;
  @ViewChild("cajalocalidad") cajaLocalidad!: ElementRef;

  constructor(
    private _cdr: ChangeDetectorRef,
    private _service: ServiceDepartamentos,
    private _router: Router
  ) {}

  async insertDept(): Promise<void> {
    let id = parseInt(this.cajaId.nativeElement.value);
    let nombre = this.cajaNombre.nativeElement.value;
    let localidad = this.cajaLocalidad.nativeElement.value;

    let dept = new Departamento(id, nombre, localidad);

    let response = await this._service.createDepartamento(dept);

    this._router.navigate(["/"]);
  }
}
```

Funcionamiento:

1. Se recogen los valores de los campos del formulario.
2. `parseInt()` convierte el identificador en un número entero.
3. Se crea un objeto `Departamento`.
4. Se llama a `createDepartamento()` del servicio.
5. Se espera la respuesta de la API.
6. Se utiliza `_router.navigate(["/"])` para volver a la página principal.

**Importante:** para utilizar `_router.navigate()`, hay que importar `Router` desde `@angular/router` e inyectarlo en el constructor.

---

## 3. UPDATE: actualizar un departamento

Para actualizar un departamento, primero se obtiene el departamento que se quiere modificar. Después, se muestran sus datos en un formulario, se recogen los cambios y se envían a la API.

### Método getDepartamento() en el servicio

Este método obtiene un departamento concreto a partir de su identificador.

```typescript
async getDepartamento(id: number): Promise<Departamento> {
  let endpoint = "api/departamentos/" + id;

  return firstValueFrom(
    this._http.get<Departamento>(this.url + endpoint)
  );
}
```

El identificador se añade al endpoint para recuperar un departamento concreto.

### Método updateDepartamento() en el servicio

```typescript
async updateDepartamento(
  departamento: Departamento
): Promise<any> {
  // Convertimos el objeto a JSON
  let json = JSON.stringify(departamento);

  // Creamos la cabecera para el tipo de contenido
  let header = new HttpHeaders()
    .set("content-type", "application/json");

  let endpoint = "api/departamentos";

  return firstValueFrom(
    this._http.put(
      this.url + endpoint,
      json,
      { headers: header }
    )
  );
}
```

Funcionamiento:

1. Recibe el departamento con los datos actualizados.
2. Convierte el objeto a JSON mediante `JSON.stringify()`.
3. Establece la cabecera `content-type: application/json`.
4. Realiza una petición HTTP PUT.
5. Envía el objeto actualizado a la API.

### Formulario HTML

Se utiliza `@if` para mostrar el formulario únicamente cuando el departamento ya está cargado.

```html
@if (departamento) {
  <div style="border: 5px solid lightblue; margin: auto; width: 50%; display: flex; flex-direction: column; padding: 10px; margin-top: 20px;">

    <h1>UPDATE DEPARTAMENTO</h1>

    <form #formUpdate="ngForm" action=""
      style="display: flex; flex-direction: column; gap: 10px;">

      <label>Id Departamento:</label>
      <input name="cajaid" #cajaid type="number"
        value="{{departamento.numero}}" disabled>

      <label>Nombre:</label>
      <input name="cajanombre" #cajanombre
        value="{{departamento.nombre}}" type="text">

      <label>Localidad:</label>
      <input name="cajalocalidad" #cajalocalidad
        type="text" value="{{departamento.localidad}}">

      <button (click)="updateDept()"
        style="background-color: lightblue; border: 1px solid lightblue; border-radius: 8px;">
        update
      </button>
    </form>
  </div>
}
```

El identificador aparece deshabilitado porque se utiliza para identificar el departamento que se modifica. El nombre y la localidad se pueden editar.

### Componente UpdateComponent

```typescript
export class UpdateComponent implements OnInit {
  public departamento!: Departamento;

  @ViewChild("cajaid") cajaId!: ElementRef;
  @ViewChild("cajanombre") cajaNombre!: ElementRef;
  @ViewChild("cajalocalidad") cajaLocalidad!: ElementRef;

  constructor(
    private _cdr: ChangeDetectorRef,
    private _service: ServiceDepartamentos,
    private _activeRouter: ActivatedRoute,
    private _router: Router
  ) {}

  async updateDept(): Promise<void> {
    let id = parseInt(this.cajaId.nativeElement.value);
    let nombre = this.cajaNombre.nativeElement.value;
    let localidad = this.cajaLocalidad.nativeElement.value;

    let dept = new Departamento(id, nombre, localidad);

    let response = await this._service.updateDepartamento(dept);

    this._router.navigate([
      "/details",
      dept.numero,
      dept.nombre,
      dept.localidad
    ]);
  }

  async ngOnInit(): Promise<void> {
    this._activeRouter.params.subscribe(async (params: Params) => {
      let id = params["id"];

      this.departamento = await this._service.getDepartamento(id);

      this._cdr.detectChanges();
    });
  }
}
```

Funcionamiento:

1. `ActivatedRoute` permite acceder a los parámetros de la ruta.
2. Se obtiene el identificador del departamento.
3. Se llama a `getDepartamento(id)` para cargar sus datos.
4. Se actualiza la vista con `detectChanges()`.
5. El usuario modifica el nombre o la localidad.
6. `updateDept()` recoge los datos y crea un objeto `Departamento`.
7. Se llama a `updateDepartamento()` para enviar los cambios.
8. Se navega a la página de detalles mediante `Router`.

Para utilizar `Params`, `ActivatedRoute` y `Router`, hay que importarlos desde `@angular/router`.

---

## 4. DELETE: eliminar un departamento

Para eliminar un departamento, se realiza una petición HTTP DELETE utilizando su identificador.

### Método borrarDepartamento() en el servicio

```typescript
async borrarDepartamento(id: number): Promise<any> {
  let endpoint = "api/departamentos/" + id;

  return firstValueFrom(
    this._http.delete(this.url + endpoint)
  );
}
```

El identificador se concatena con el endpoint para indicar qué departamento se quiere eliminar.

### Botón en el HTML

En la tabla de departamentos se añade un botón que llama a `eliminar()` y le pasa el identificador del departamento.

```html
<td style="border: 3px solid lightblue; padding-left: 5px;">
  <button (click)="eliminar(departamento.numero)"
    style="border: 3px solid lightblue; border-radius: 5px; margin: 15px; background-color: lightblue;">
    🗑️
  </button>
</td>
```

### Componente HomeComponent

```typescript
export class HomeComponent implements OnInit {
  public departamentos!: Array<Departamento>;

  constructor(
    private _service: ServiceDepartamentos,
    private _cdr: ChangeDetectorRef
  ) {}

  async eliminar(id: number) {
    let response = await this._service.borrarDepartamento(id);

    await this.cargar();
  }

  async cargar() {
    this.departamentos = await this._service.getDepartamentos();

    this._cdr.detectChanges();
  }

  async ngOnInit(): Promise<void> {
    await this.cargar();
  }
}
```

Funcionamiento:

1. Se pulsa el botón de eliminar de un departamento.
2. Se llama a `eliminar()` pasando su identificador.
3. El componente llama a `borrarDepartamento()` del servicio.
4. El servicio realiza la petición HTTP DELETE.
5. Se vuelve a llamar a `cargar()` para obtener la lista actualizada.
6. `detectChanges()` actualiza la vista.

El método `cargar()` evita repetir el código de obtención de los departamentos y permite reutilizarlo tanto al iniciar el componente como después de eliminar un registro.

---

## 5. Resumen de las operaciones CRUD

| Operación | Método HTTP | Método del servicio | Función |
|---|---|---|---|
| GET | `GET` | `getDepartamentos()` | Obtener todos los departamentos. |
| CREATE | `POST` | `createDepartamento()` | Crear un departamento. |
| UPDATE | `PUT` | `updateDepartamento()` | Actualizar un departamento. |
| DELETE | `DELETE` | `borrarDepartamento()` | Eliminar un departamento por su ID. |

### Conceptos importantes

- **HttpClient:** permite realizar peticiones HTTP desde Angular.
- **Observable:** objeto utilizado por Angular para representar datos asíncronos.
- **firstValueFrom():** convierte un Observable en una promesa.
- **async/await:** permite trabajar con operaciones asíncronas de forma más sencilla.
- **HttpHeaders:** permite configurar las cabeceras HTTP.
- **JSON.stringify():** convierte un objeto JavaScript en una cadena JSON.
- **ElementRef:** permite acceder a un elemento del HTML.
- **@ViewChild:** permite obtener referencias a elementos del HTML desde el componente.
- **ChangeDetectorRef:** permite controlar la detección de cambios.
- **ActivatedRoute:** permite acceder a los parámetros de la ruta actual.
- **Router:** permite navegar entre rutas mediante código.
- **OnInit:** permite ejecutar código durante la inicialización del componente.

**Idea clave:** el componente recoge los datos y controla la interfaz; el servicio realiza las peticiones HTTP a la API.
