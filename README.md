Crear una aplicación para buscar películas

API a usar:

- https://www.omdbapi.com/
- API_KEY: 4287ad07

Requerimientos:

- Necesita mostrar un input para buscar la película y un botón para buscar.
- Lista las películas encontradas y muestra el título, año y poster.
- Haz que las películas se muestren en un grid responsive.

Primera iteración:

- Evitar que se haga la misma búsqueda dos veces seguidas.
- Haz que la búsqueda se haga automáticamente al escribir.
- Evita que se haga la búsqueda continuamente al escribir (debounce)


## Iniciando proyecto
- ``npm create vite@latest``
- **Opciones**: _titulo_="buscador-peliculas", _framework_="React", _variant_="JavaScript + SWC"
- Ejecutamos ``npm install`` o ``pnpm install`` ( más rápido)

___
- ***eslinter***
- Instalamos el ``eslinter``

```bash
npm install standard -D      
```

- En package.json añadimos la configuración de eslint

```json
"eslintConfig": {
    "extends": "./node_modules/standard/eslintrc.json"
  }
```

- Vamos a la ruta del extends en nuestro proyecto y en el ``eslintrc.json`` escribimos la regla para que no marque como error los punto y comas.

```json
{
  "extends": ["standard", "standard-jsx"],
  "rules": {
    "semi": ["error", "always"]
  }
}
```

- En el ``settings.json`` de VSC añadimos una linea para que se auto formatee al guardar:

```json
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
```

Si junto al _package.json_ se genera un ``eslintrc.cjs`` , lo eliminamos.

___
- ***Water.css***

https://watercss.kognise.dev/

Usaremos el Framework Water.css que nos estilará un poco la página fácilmente

![Pasted image 20240225103401](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/830e70ec-460b-45a0-8499-31119c8bc43e)


Abrimos la ruta y copiamos todo el css en el _index.css_ del proyecto

###  Mostrar un input para buscar la película y un botón para buscar
Estructura básica para la aplicación de búsqueda de películas. Tenemos un encabezado con un título y un formulario para ingresar el nombre de una película y un botón para buscar. Además,  un área (`<main>`) donde mostrar los resultados de la búsqueda.

![Pasted image 20240225105612](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/bd159a38-c2aa-47f2-82da-098d9f73a08d)

![Pasted image 20240225105640](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/7fdcb3dc-480a-49d4-9340-1ba0128b6ef5)

### Revisar la API 
https://www.omdbapi.com/

Como tenemos que listar las películas encontradas y mostrar el título, año y poster vemos que nos hará falta la información de esta parte:

![Pasted image 20240226074718](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/c6b9b010-930c-4bfb-944e-1b219244e264)

Probaremos el resultado de una petición con :

![Pasted image 20240226074820](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/d958d8df-d51a-4e83-8d21-4a03e99a1484)

Donde tendremos que completar los siguientes datos:
- **apikey** = La que que nos proporcionaron al principio del proyecto ``4287ad07``
- **s** = Que vimos que es un parámetro requerido en el _By Search_ anterior
- **Título** = Y como vimos en la _descripción_ la s hace referencia a un _título_ del que realizaremos la búsqueda

Por lo tanto la petición debería ser algo así:
```bash
http://www.omdbapi.com/?apikey=4287ad07&s=avatar
```

Una buena práctica es crear una carpeta mock y guardar un json con el resultado de una petición correcta y otro json con una petición incorrecta para conocer la estructura.

![Pasted image 20240226080437](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/c59c6609-274f-4978-acde-e7c23c564272)


### Visualización provisional con un mock

- Forma básica
![Pasted image 20240226103423](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/6e510449-f301-447b-a21b-84799fd04532)

- Otra forma de hacerlo separando los renderizados del html

![Pasted image 20240226104250](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/d45ab086-b793-4bde-a546-acf142eda2c9)

#### Forma más optima y recomendada , con componentes
- Creamos una carpeta llamada _components_ y un archivo llamado _Movies.jsx_

![Pasted image 20240226113554](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/225c4970-324e-402a-9b6f-8ad4494610c9)

![Pasted image 20240227073207](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/5af6b1c6-7958-44fb-846c-da39c36d769b)


#### Evitar depender del contrato de la API 
La modificación introducida en `App.jsx` y `Movies.jsx` tiene como objetivo principal desacoplar la estructura de datos específica de la API (o en este caso, del mock de datos) del resto de la aplicación. Esto se logra mapeando los datos brutos a un formato más controlado y predecible dentro de `App.jsx`, y luego utilizando este formato estandarizado en el componente `Movies` y sus subcomponentes.

![Pasted image 20240227074947](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/b14a9f9f-e910-4a40-927b-8513d8b27d0d)

#### Crear un custom hook useMovies
La introducción de `useMovies`, un hook personalizado en React, es un paso adelante hacia una mejor separación de la lógica y la presentación en tu aplicación. Esta modificación permite encapsular la lógica de obtención y mapeo de las películas en un solo lugar, lo que hace que el componente `App` sea aún más limpio y enfocado en la estructura del UI.

![Pasted image 20240227080430](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/e583c34a-3270-40e1-9556-d7561e0ec5a4)

- Separación del custom hook en archivo separado

![Pasted image 20240227095732](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/0d6d28a9-9631-48d8-abb6-ad4152f64df4)


### Manejar formularios y hook useRef
`useRef` es un Hook de React que te permite acceder a una referencia de un elemento DOM o almacenar un valor mutable que no causará una nueva renderización del componente cuando se actualice. La principal diferencia entre `useRef` y `useState` es cómo React maneja los cambios en sus valores:

- **`useState`** proporciona un valor y una función para actualizar ese valor. Cuando utilizas la función de actualización, React vuelve a renderizar el componente para reflejar el nuevo estado. Esto es útil para datos que, cuando se cambian, deben causar una actualización visual en la interfaz de usuario.
    
- **`useRef`**, por otro lado, devuelve un objeto mutable con una propiedad `.current`. Este objeto persiste durante toda la vida del componente sin desencadenar una nueva renderización cuando su contenido cambia. Esto lo hace ideal para almacenar valores que necesitas conservar a lo largo de renderizaciones pero que no deberían causar efectos visuales cuando se modifican, como una referencia a un elemento del DOM o el almacenamiento de valores previos de props o estado para comparaciones.

___

La última modificación en el código de React introduce el manejo del formulario utilizando el hook `useRef` de React para acceder al valor del campo de entrada (input) del formulario sin necesidad de manejar el estado del componente para dicho valor.

![Pasted image 20240227103730](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/2ed0a4a6-82f4-430c-b6bd-7555cb8d2dc9)

#### Hacer lo mismo de forma nativa sin utilizar el hook useRef
Esta nueva implementación maneja el formulario sin utilizar `useRef` de React, sino a través de la API nativa `FormData`, que es una forma de acceder y manipular los datos del formulario directamente desde el DOM.

![Pasted image 20240227110535](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/1adc139c-2d9d-410b-849d-6c89b1f4add5)

#### Versión reducida y para recuperar varios inputs a la vez
La nueva forma de manejar el formulario en el código utiliza el objeto `FormData` y el método `Object.fromEntries` para recolectar y manipular los datos del formulario de una manera más general y escalable.

![Pasted image 20240227112020](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/960c5b4a-6d7a-40a0-ba47-6ac492cec7d3)

![Pasted image 20240227111039](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/82d0fcd8-7292-4526-8189-c87c810d268f)

- Resultado final para recuperar los datos de forma nativa

![Pasted image 20240227112542](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/3f0ccad8-adbe-4306-858d-98b0c9a86213)


#### Controlar formularios con React

![Pasted image 20240228071731](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/11a2bb5f-8832-42da-8c5c-f6d61910e811)


#### Validaciones

- Sin useEffect 
![Pasted image 20240228073901](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/aed5e8a3-530e-4614-954c-26fddbe1c015)

- Con useEffect

![Pasted image 20240228074532](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/0cc35597-a535-432a-b500-e512ad2a71e0)

- Añadir un estilo al marco del input cuando se valide un error

![Pasted image 20240228075323](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/ee7dd8a7-72d5-456e-b112-ab5d005cdfc4)


##### Extraer validaciones en un hook personalizado

![Pasted image 20240228100226](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/e2e31be6-2e55-4134-bc25-c9c9e70bfdcb)



##### useRef para diferenciar comportamiento inicial
La utilización de `useRef` junto con `useEffect` en este contexto tiene un propósito específico: controlar y diferenciar el comportamiento inicial del componente de sus actualizaciones subsiguientes.

![Pasted image 20240228105703](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/c0f3ab02-a18d-4727-a676-fedd8fbab998)

![Pasted image 20240228105804](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/0feead35-7ab1-4679-ade8-8840561411a5)


### Estilar como grid los resultados

![Pasted image 20240229072701](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/9b54bf8c-5f83-4157-8f38-c0bba05ac4c5)

![Pasted image 20240229072749](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/2bf9cc4c-ba85-4a9b-a86b-d23025405136)


### Fetching de datos

#### Función para obtener las películas

![Pasted image 20240229080351](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/827ef3e8-d6d0-4d47-aab4-ed451f2b154e)


#### fetch de los datos

![Pasted image 20240229101703](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/cc64323b-0a83-4133-b0cd-823d6946d5b8)

### Extraer lógica de servicio 

Los cambios realizados en tu aplicación de búsqueda de películas reflejan una evolución hacia una estructura más modular y profesional, mejorando la separación de responsabilidades, la gestión del estado y la experiencia del usuario durante las solicitudes de red. Aquí tienes un resumen de las modificaciones clave:

 1. ***Separación de la Lógica de Servicio (`movies.js`):***

- **Extracción de la Lógica de API**: Has movido la lógica para llamar a la API de OMDB a un archivo separado (`movies.js`), definiendo una función `searchMovies` que realiza la búsqueda asincrónica de películas. Esto mejora la separación de responsabilidades, aislando la interacción con la API del resto de la lógica de la UI.
- **Manejo de Errores y Formato de Datos**: La función `searchMovies` maneja tanto la llamada a la API como posibles errores y formatea los datos recibidos antes de retornarlos, lo que simplifica el trabajo del componente que consume estos datos.

2. ***Mejoras en el Custom Hook (`useMovies.js`):***

- **Integración con el Servicio de Películas**: Ahora `useMovies` utiliza `searchMovies` para obtener las películas, lo que centraliza la lógica de fetching de datos y permite reutilizar este hook en diferentes componentes si fuera necesario.
- **Gestión de Estado de Carga y Errores**: Has añadido estados para manejar la carga (`loading`) y los errores (`error`) durante la solicitud de datos, lo que permite proporcionar feedback en tiempo real al usuario sobre el estado de su búsqueda.

 3. ***Actualización del Componente Principal (`App.jsx`):***

- **Feedback de Carga**: Utilizas el estado `loading` para mostrar un mensaje de "Cargando..." mientras se espera la respuesta de la API. Esto mejora la experiencia del usuario, proporcionando una indicación visual de que su solicitud está siendo procesada.
- **Uso de Ternarias para la Gestión de Contenido**: Empleas una expresión ternaria para alternar entre mostrar el mensaje de carga y los resultados de la búsqueda (o la ausencia de estos), dependiendo del estado de `loading`.

 4. ***Componentes de Presentación (`Movies.jsx`):***

- **Mantenimiento de la Estructura**: Los componentes de presentación permanecen mayormente sin cambios, focalizándose en mostrar la lista de películas o un mensaje de que no hay resultados, dependiendo de los datos recibidos. Esto demuestra una buena práctica en React: mantener los componentes de UI lo más agnósticos posible respecto a la lógica de negocio o de datos.

![Pasted image 20240229110256](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/02847b5d-9c61-43b4-81d5-e814bf6f3288)


### Evitar la misma búsqueda con useRef

![Pasted image 20240301072344](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/effceae3-73d1-4e25-9b1d-e7a5c5bea0e6)

### useMemo (ordenar películas por año)

![Pasted image 20240301075111](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/ca26f1dc-feb7-4a0e-a727-90b7c1d094cd)

 `useMemo`

- **Propósito:** `useMemo` se utiliza para memorizar un valor calculado. Es decir, permite evitar cálculos costosos en cada render si las dependencias no han cambiado. `useMemo` solo recalcula el valor memorizado cuando una de las dependencias ha cambiado.
- **Uso:** Es útil cuando tienes operaciones computacionalmente costosas que no necesitas ejecutar en cada render. Por ejemplo, cálculos pesados, filtrado de grandes conjuntos de datos, etc.

 `useEffect`

- **Propósito:** `useEffect` se utiliza para ejecutar efectos secundarios en tu componente. Los efectos secundarios pueden incluir operaciones que afectan a componentes fuera del ámbito del componente React actual, como solicitudes de datos, suscripciones, o manualmente modificar el DOM de manera imperativa.
- **Uso:** Es útil para operaciones que necesitan ser realizadas después de que el DOM se haya actualizado o para operaciones que deben correr después de la renderización pero no afectan el resultado de la renderización directamente, como cargar datos de una API.

### useMemo para recordar búsqueda ya realizada 
El uso de `useMemo` en `useMovies.js` para definir `getMovies` es un caso interesante de cómo React optimiza el rendimiento y maneja funciones costosas o efectos secundarios.

![Pasted image 20240302073148](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/9f62c15f-df3d-4ea1-9d32-04f199310aff)


### useCallback
- Exactamente igual que useMemo solo que no es necesario pasarle como parámetro una función como en useMemo que sería:

```js
// useMemo
  const getMovies = useMemo( funcion(ejcucion), [] )

// useCallback
  const getMovies = useCallback( ejecucion, [] )
```


![Pasted image 20240302073841](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/1e1ecd49-9535-41fb-8c53-f643e999dd6e)

Si es para una **función** usar _useCallback_, si es para un **valor** usar _useMemo_ .

### Búsqueda automática al escribir

- Enviando el _newSearch_ en el _getMovies_  conseguiríamos que se renderizara el componente cada vez que lo enviásemos al ser el search un estado.

![Pasted image 20240302101521](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/b2f8a1c7-2c39-4b77-ad9f-16cf63f48b6c)

#### debounce

Se podría usar una librería , por ejemplo:

https://github.com/angus-c/just

install
```bash
pnpm install just-debounce-it -E 
```

![Pasted image 20240302103506](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/99476438-0785-4176-a59d-acf0dcbfba8b)


***¿Qué hace `debounce`?***

El `debounce` es una técnica de control que limita la tasa a la que una función puede dispararse. Cuando se usa en una función que se activa repetidamente (por ejemplo, un oyente de eventos de tecleo), `debounce` asegura que la función solo se ejecute una vez después de que las llamadas repetidas se detengan durante un período determinado. Es útil para reducir el número de llamadas a funciones que pueden ser costosas en términos de rendimiento o que disparan cambios significativos en la UI, como las consultas de búsqueda.

En este caso, `debounce` se utiliza para asegurar que la función `getMovies` solo se llame 300 milisegundos después de que el usuario haya dejado de escribir. Esto previene que la aplicación realice una solicitud de búsqueda por cada tecla presionada, lo cual podría resultar en una sobrecarga de solicitudes y una experiencia de usuario pobre.

***¿Cómo funciona `useCallback` aquí?***

`useCallback` es un Hook que devuelve una versión memorizada de la función pasada a él, lo que ayuda a evitar la creación innecesaria de funciones en cada renderizado. Esto es especialmente útil cuando pasas funciones como props a componentes hijos optimizados o cuando dependes de la igualdad de referencia para evitar renderizados innecesarios.

En este fragmento de código:

```jsx
const debounceGetMovies = useCallback(debounce(search => {     
	getMovies({ search });   
	}, 300)
	, [getMovies]
);
```

`useCallback` envuelve la función `debounce`, que a su vez envuelve la función `getMovies`. La dependencia `[getMovies]` significa que `debounceGetMovies` solo se recreará si `getMovies` cambia, lo cual no debería suceder a menudo dado cómo se define `getMovies` en este contexto (es probablemente también una función memorizada o que no cambia entre renderizados). Esto asegura que el debouncing funcione correctamente y que no se pierda el beneficio del debouncing por la creación de nuevas funciones en cada render.

***Integración con `handleChange`***

Cuando el usuario escribe en el campo de búsqueda, el evento `onChange` llama a `handleChange`, que actualiza el estado `search` y llama a `debounceGetMovies` con el nuevo valor de búsqueda. Gracias al debouncing, `getMovies` solo se invocará después de que el usuario haya dejado de escribir durante 300 milisegundos, lo que mejora la eficiencia al reducir el número de llamadas a la API y proporciona una experiencia de usuario más fluida.

![Pasted image 20240302110046](https://github.com/Mileccc/Buscador_peliculas/assets/121825748/2980a525-33f2-4020-a1a0-6046c74b4686)















