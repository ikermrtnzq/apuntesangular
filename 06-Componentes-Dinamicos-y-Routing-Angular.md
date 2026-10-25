
# COMPONENTES DINÁMICOS Y ROUTING EN ANGULAR

## 1. Crear componentes dinámicos

Para generar un componente mediante Angular CLI, utilizamos el siguiente comando:

```bash
ng g c components/cineComponent
```

A veces, Angular no incluye automáticamente el componente en `app-module.ts`, por lo que debemos comprobarlo.

Si queremos crear un componente en otra ruta, fuera de la carpeta habitual, podemos utilizar:

```bash
ng g c cineComponent --path src/components --skip-import
```

## 2. Routing en Angular

El routing permite navegar entre diferentes componentes de nuestra aplicación.

Para configurar las rutas, debemos declarar los componentes necesarios en `app-module.ts` e importar el sistema de routing.

### 2.1. Configurar app-module.ts

```typescript
import { appRoutingProvider, routing } from './app.routing';

@NgModule({
    declarations: [
        App,
        HomeComponent,
        MusicaComponent,
        CineComponent,
        NotFoundComponent
    ],
    imports: [
        BrowserModule,
        routing
    ],
    providers: [
        provideBrowserGlobalErrorListeners(),
        appRoutingProvider
    ],
    bootstrap: [App],
})
```

## 3. Crear el archivo app.routing.ts

En este archivo definimos las rutas de la aplicación y el componente que se mostrará en cada una.

```typescript
import { Routes, RouterModule } from "@angular/router";
import { ModuleWithProviders } from "@angular/core";

import { HomeComponent } from "./components/home-component/home-component";
import { CineComponent } from "./components/cine-component/cine-component";
import { MusicaComponent } from "./components/musica-component/musica-component";
import { NotFoundComponent } from "./components/not-found-component/not-found-component";

const appRoutes: Routes = [
    { path: "", component: HomeComponent },
    { path: "cine", component: CineComponent },
    { path: "musica", component: MusicaComponent },
    { path: "**", component: NotFoundComponent }
];

export const appRoutingProvider: any[] = [];

export const routing: ModuleWithProviders<any> =
    RouterModule.forRoot(appRoutes);
```

Las rutas definidas son:

- `""`: muestra el componente `HomeComponent`.
- `"cine"`: muestra el componente `CineComponent`.
- `"musica"`: muestra el componente `MusicaComponent`.
- `"**"`: muestra `NotFoundComponent` cuando ninguna de las otras rutas coincide.

**Importante:** en las rutas no debemos poner `/` al principio del valor de `path`.

## 4. Crear un menú de navegación

Podemos crear un componente de menú para navegar entre las distintas rutas utilizando `routerLink`.

```html
<ul>
    <li><a [routerLink]="['/']">Home</a></li>
    <li><a [routerLink]="['/cine']">Cine</a></li>
    <li><a [routerLink]="['/musica']">Musica</a></li>
</ul>
```

## 5. Mostrar las rutas en app.html

Para mostrar el menú y el componente correspondiente a la ruta actual, utilizamos `router-outlet`.

```html
<div>
    <app-menu-component></app-menu-component>
    <router-outlet></router-outlet>
</div>
```

- `app-menu-component`: muestra el menú de navegación.
- `router-outlet`: muestra el componente correspondiente a la ruta seleccionada.
