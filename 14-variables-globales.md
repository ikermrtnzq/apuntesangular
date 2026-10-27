# 14. Variables globales en Angular

## 1. Crear variables globales

Para utilizar variables globales en el proyecto, creamos un archivo llamado `global.ts` dentro de la carpeta `src/app`.

En este archivo podemos declarar valores que necesitemos utilizar desde diferentes partes de la aplicación, como la URL de una API.

Archivo: `src/app/global.ts`

```typescript
export var Global = {
  urlApiPersonas:
    "https://servicioapipersonasmvcpgs.azurewebsites.net/api/personas"
};
```

En este ejemplo, `Global` es un objeto que contiene la propiedad `urlApiPersonas`, con la dirección de la API de personas.

## 2. Importar las variables globales

Para acceder a estas variables desde un servicio, importamos el objeto `Global`:

```typescript
import { Global } from "../global";
```

La ruta del import dependerá de dónde esté situado el archivo que lo utiliza.

## 3. Utilizar las variables globales en un servicio

En lugar de escribir directamente la URL en cada método, podemos utilizar la propiedad definida en `Global`.

```typescript
getPersonas(): Observable<any> {
  let url = Global.urlApiPersonas;

  return this._http.get(url);
}
```

Así, el servicio utiliza la URL almacenada en el archivo `global.ts`.

## 4. Ventajas

- Centralizamos valores que se utilizan en diferentes partes del proyecto.
- Evitamos repetir la misma URL en varios métodos.
- Si necesitamos cambiar la dirección de la API, podemos modificarla en un único archivo.

## 5. Resumen

- Creamos `global.ts` dentro de `src/app`.
- Exportamos un objeto con las variables globales.
- Importamos ese objeto desde los archivos que lo necesiten.
- Utilizamos sus propiedades para acceder a los valores compartidos.
