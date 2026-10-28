
# RUTAS CON PARAMETROS

Para mostrar Detalle añadimos en el home un botón que nos mande:

```html
@for (departamento of departamentos; track departamento) {
   <tr style="border: 1px solid lightblue;">
     <td style="border: 3px solid lightblue;padding-left: 5px;">
        {{departamento.numero}}
     </td>
     <td style="border: 3px solid lightblue; padding-left: 5px;">
        {{departamento.nombre}}</td>
     <td style="border: 3px solid lightblue;padding-left: 5px;">
        {{departamento.localidad}}
     </td>
     <td style="border: 3px solid lightblue;padding-left: 5px;"><button style="border: 3px solid lightblue;border-radius: 5px; margin: 15px; background-color: lightblue;">
       <a [routerLink]="['/details/', departamento.numero, departamento.nombre, departamento.localidad]" style="text-decoration: none; color: black">detalle</a></button></td>
    </tr>
}
```

Para que esa ruta funcione en el router hemos añadido este path:

```typescript
{path : "details/:num/:nombre/:localidad", component : DetallesDepartamentoComponent},
```

Para recibir los datos en el component al que pasamos los datos por parámetros utilizamos ActivatedRoute y params:

```typescript
export class DetallesDepartamentoComponent implements OnInit {
 public departamento! : Departamento;

 constructor(
  private _activateRoute: ActivatedRoute,
  private _cdr : ChangeDetectorRef
 ) {

 }

 ngOnInit(): void {
   this._activateRoute.params.subscribe((params: Params) => {
     let id = parseInt(params['num']);
     let nombre = params['nombre'];
     let localidad = params['localidad'];

     this.departamento = new Departamento(id, nombre, localidad);
     this._cdr.detectChanges()
   })
 }
}
```
