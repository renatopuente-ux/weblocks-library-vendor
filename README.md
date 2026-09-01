# weblocks library.js — copia vendorizada

Copia de `library.js` de **Weblocks**, guardada para versionarla y tener una referencia estable
de qué versión se está usando en los prototipos.

## ⚠️ Estado de licencia — leer antes de mover este repo

**No se encontró ninguna licencia.** Ni en el archivo (sin cabecera de copyright, sin aviso de
licencia) ni publicada en el sitio: `weblocks.io/LICENSE`, `/license` y `/terms` devuelven 404.

Sin licencia expresa, por defecto rige *todos los derechos reservados*. Por eso **este repositorio
es privado**. Antes de hacerlo público habría que confirmar con Weblocks que su licencia de uso
permite redistribuir el script.

Lo que sí es seguro: usarlo dentro de un proyecto propio si eres cliente de Weblocks, y tener esta
copia privada para saber exactamente qué versión corre en producción.

**Excepción:** el archivo lleva embebida una copia minificada de **js-cookie 2.2.1**, que sí es MIT
(`/npm/js-cookie@2.2.1/src/js.cookie.js`, minificada por jsDelivr). Esa parte se podría usar y
redistribuir por separado sin problema.

## Procedencia

| | |
|---|---|
| Origen | `https://weblocks.io/library.js` |
| Descargado | 1 de septiembre de 2026 |
| Tamaño | 9.538 bytes |
| `sha256` | `90380b1ee45a995e94d756e5c6cd66f4ff80a4e66fcccaf6747035286318da7b` |
| Source map | Referenciado por el archivo pero **404** en el servidor |

El archivo no está versionado por Weblocks: no expone número de versión. El `sha256` es la única
forma de saber si cambió. Para comprobar si hay una versión nueva, volver a descargarlo y comparar
el hash contra el de esta tabla.

## Qué trae

Utilidades para formularios de Webflow, más dos widgets. Todo depende de **jQuery**.

| Función | Para qué |
|---|---|
| `updateValueInInput` / `updateValueInInputData` | Escribe un valor en un campo, buscándolo por `name` o por `data-name`. Maneja radios y checkboxes aplicando las clases de estado de Webflow (`w--redirected-checked`) |
| `getValueFromInput` / `getValueFromInputData` | Lee el valor de un campo. Convierte a número si aplica, y para checkboxes devuelve el booleano de `checked` |
| `numberWithCommas` | Inserta un separador de miles |
| `replaceInText` | Reemplaza todas las ocurrencias de una cadena |
| `getUrlParameter` | Lee un parámetro del query string |
| `varToString` | Devuelve el nombre de la primera clave de un objeto |
| `TimeAgo` | Convierte fechas en texto tipo "hace 3 días" |
| `Timer` | Cuenta regresiva hacia una fecha, actualizada cada segundo |
| `Cookies` | js-cookie 2.2.1 embebido (MIT) |

## Hallazgos de revisión

Encontrados al leerlo. No los corregí: el archivo está **sin modificar**, y tocarlo aquí haría que
dejara de coincidir con lo que sirve Weblocks. Anotados para que no sorprendan.

1. **`self = this` sin declarar**, en `TimeAgo` y en `Timer`. Crea una global implícita y falla bajo
   `"use strict"`. Además, si instancias los dos widgets en la misma página, **el segundo pisa el
   `self` del primero**.
2. **`template = {...}` sin declarar**, dentro de `render()`. Otra global implícita.
3. **`timeSince` puede lanzar excepción.** Usa `.find(i => i.seconds < seconds)`; si transcurrió
   menos de 1 segundo no encuentra intervalo y `interval.seconds` revienta sobre `undefined`.
4. **Dependencia global no declarada:** `getValueFromInput` y `getValueFromInputData` leen
   `clickedRadioButtonValue`, una variable que este archivo nunca define. Tiene que existir en otro
   script cargado antes, o los radios devuelven `undefined`.
5. **`TimeAgo` con varias fechas escribe lo mismo en todas.** `wb_outPut.html(...)` aplica a todos
   los elementos del selector, no al del índice que se está recorriendo.
6. Requiere **jQuery** cargado antes (28 llamadas a `$`).

## Actualizar

1. Descargar `https://weblocks.io/library.js`.
2. Comparar su `sha256` con el de la tabla de procedencia.
3. Si cambió: reemplazar el archivo, actualizar la tabla con la fecha y el hash nuevos, y revisar si
   los hallazgos de arriba siguen aplicando.
