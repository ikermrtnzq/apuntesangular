# 15. Variables de entorno en Angular

## 1. ¿Qué son las variables de entorno?

Las variables de entorno son similares a las variables globales, pero tienen una ventaja: permiten tener configuraciones diferentes para desarrollo y producción.

Angular utiliza por defecto el archivo de desarrollo. Una vez realizado el `build` para producción, se utilizan los valores configurados para ese entorno, según la configuración del proyecto.

Esto resulta útil, por ejemplo, para utilizar diferentes direcciones de API en desarrollo y en producción.

## 2. Crear los archivos de entorno

Para generar los archivos de entorno utilizamos el siguiente comando:

```bash id="5v1jyk"
ng g environments
```

Este comando crea dos archivos:

- `environment.development.ts`: contiene la configuración del entorno de desarrollo.
- `environment.ts`: contiene la configuración del entorno de producción.

## 3. Configurar el entorno de desarrollo

En el archivo `environment.development.ts` podemos definir la URL de la API:

```typescript id="w0o6m1"
export const environment = {
  urlApiPersonas:
    "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas"
};
```

## 4. Configurar el entorno de producción

En el archivo `environment.ts` definimos la configuración que se utilizará en producción:

```typescript id="m5l0d8"
export const environment = {
  urlApiPersonas:
    "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas"
};
```

Los dos archivos tienen la misma estructura, pero pueden contener valores diferentes.

Por ejemplo, podemos configurar una URL para desarrollo y otra para producción.

## 5. Utilizar las variables de entorno en un servicio

Primero importamos `environment` desde el archivo correspondiente:

```typescript id="k8c2p3"
import { environment } from "../../environments/environment.development";
```

Después utilizamos la propiedad definida en el archivo:

```typescript id="h9s4x7"
getPersonas(): Observable<any> {
  let url = environment.urlApiPersonas;

  return this._http.get(url);
}
```

Así evitamos escribir directamente la URL de la API dentro del método.

**Importante:** el archivo que se importa debe coincidir con el entorno que queremos utilizar. En un proyecto Angular con configuración de reemplazo de archivos, normalmente se importa `environment` desde `environment.ts` y Angular selecciona el archivo correspondiente al construir la aplicación.

## 6. Ventajas

- Permiten separar la configuración de desarrollo y producción.
- Facilitan cambiar las URL de las API según el entorno.
- Evitan repetir valores de configuración en diferentes servicios.
- Hacen más sencillo preparar la aplicación para su despliegue.

## 7. Resumen

- Creamos los archivos con `ng g environments`.
- `environment.development.ts` contiene la configuración de desarrollo.
- `environment.ts` contiene la configuración de producción.
- Importamos `environment` en el servicio.
- Accedemos a sus propiedades, por ejemplo, `environment.urlApiPersonas`.
