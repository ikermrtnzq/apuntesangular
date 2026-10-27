# 17. CRUD API: GET y POST

## 1. GET: obtener departamentos

En el servicio se crea un método asíncrono para obtener la lista de departamentos desde la API.

```typescript
async getDepartamentos(): Promise<Departamento[]> {
  let endpoint = "api/departamentos";

  return firstValueFrom(
    this._http.get<Departamento[]>(this.url + endpoint)
  );
}
```

El método realiza una petición HTTP GET y devuelve una promesa con un array de objetos `Departamento`.

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

## 2. POST: enviar datos a la API

Para realizar una petición POST, los apuntes indican que hay que enviar el objeto con su información y establecer la cabecera `Content-Type`.

### Importar HttpHeaders

En el servicio se importa `HttpHeaders` para configurar
