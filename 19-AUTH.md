# AUTH

En algunas APIS es necesario tener un token para acceder a los datos. Se debe enviar en el header Authorization: bearer TOKEN

## Ejemplo de POSTMAN

Primero por medio de un form introducimos las credenciales:

```html
<form #formLogin="ngForm" action="" class="d-flex flex-column gap-2">
  <label for="" class="form-label">Nombre: </label>
  <input name="cajanombre" #cajanombre type="text" class="form-control">
  <label for="" class="form-label">Contraseña: </label>
   <input name="cajacontraseia" #cajacontrasenia type="password" class="form-control">
   <button (click)="login()" class="btn btn-info text-white mt-2">
      Login
   </button>
</form>
```

Luego recibimos esas credenciales en el component y hacemos la llamada al service y le pasamos las credenciales, este devolverá el token así que lo guardamos en el storage:

```typescript
export class LoginComponent {
 @ViewChild("cajanombre") cajaNombre! : ElementRef;
 @ViewChild("cajacontrasenia") cajacontrasenia! : ElementRef;

    constructor(
     private _cdr : ChangeDetectorRef,
     private _service : ServiceEmpleados,
     private _router: Router,
    ){}
    async login(): Promise<void>{
     let nombre = this.cajaNombre.nativeElement.value;
     let pwd = this.cajacontrasenia.nativeElement.value;
     let credenciales = new Credencial(nombre, pwd)
     let response = await this._service.postLogin(credenciales)
     console.log(response);
     sessionStorage.setItem("token", response.response);
     this._router.navigate(["/miperfil"])
    }
}
```

En el service las recibimos y hacemos la petición:

```typescript
async postLogin(credenciales : Credencial): Promise<any>{
  let endpoint = "auth/login";
  let json = JSON.stringify(credenciales);
  let header = new HttpHeaders().set("content-type","application/json");
  return firstValueFrom(
     this._http.post(this.url+endpoint, json, {headers: header})
   )
}
```

Luego si queremos hacer peticiones en las que pasamos la credencial creamos un header de tipo athorization y lo ponemos ahí:

Primero debemos obtener el token:

```typescript
token = sessionStorage.getItem("token");
```

Luego lo usamos:

```typescript
async obtenerPerfil() : Promise<Empleado>{
  //creamos un objeto header para el type
  let header= new HttpHeaders().set("authorization", "bearer "+ this.token);
  let endpoint= "api/Empleados/PerfilEmpleado";
  return firstValueFrom(
     this._http.get<Empleado>(this.url+endpoint, {headers: header})
  )
}
```
