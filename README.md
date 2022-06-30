# Proyecto: Vector digital WEB

## Índice principal de la aplicación web
---

### A) [Descripción general del proyecto](#item1)
### B) [Descarga de código](#item2)
### C) [Ramas principales](#item3)
### D) [Instalación e inicialización](#item4)
### E) [Vista general de las carpetas y archivos](#item5)
### F) [Definición de las carpetas y archivos de Redux-Saga](#item6)
### G) [Flujo para agregar API's](#item7)
### H) [Implementación del Store y estados en componentes o containers](#item8)
### I) [Agregar una nueva ruta al proyecto](#item9)

<br>

<a name="item1"></a>
### A) Descripción general del proyecto
---

Este proyecto contiene el código del lado del frontend para la plataforma [e-Vector](https://e-vector.com.mx/ "Plataforma e-Vector"), usa principalmente [React](https://reactjs.org/ "Sitio oficial de React") para la construcción de toda la interfaz de usuario y algunas otras librerias de apoyo de las que destacan [tailwindcss](https://tailwindcss.com/ "Sitio oficial de tailwindcss.") para los estilos de las vistas y [Redux-Saga](https://redux-saga.js.org/ "Sitio oficial de Redux-Saga.") para el manejo de la información a travez de todo el flujo de la aplicación.
<br>
A continuacion se mencionan algunas consideraciones para que el proyecto funcione de la manera correcta o bien se tenga una idea general del mismo.

[Subir](#top)

<a name="item2"></a>
### B) Descarga del código
---

1. Para la descarga del proyecto primero debemos considerar tener acceso al mismo, es decir pedir al jefe directo registre nuestra cuenta de github a su organización en este caso: **github.com/VectorCB** 
2. Una vez registrados descargamos el repositorio mediante https o ssh según corresponda la configuración del equipo que manejemos:

   - (https) $ `git clone https://github.com/VectorCB/vectorDigital-web.git`
   - (ssh) $ `git clone git@github.com:VectorCB/vectorDigital-web.git`
  
3. No olvides pedir a tu jefe directo o compañeros el archivo `.env` sino el proyecto no va a correr como es debido
<br>

[Subir](#top)

<a name="item3"></a>
### C) Ramas principales
---
Hay dos ramas importantes en el proyecto: _master_ y _develop_.

En **master** se encuentra el código relacionado a todo lo mostrado en el sitio beta.e-vector y producción.

En **develop** esta lo que se muestra en el link de develop.e-vector, aquí se encuentran los secrets de develop.

_NOTA:_ **Develop**  se debe de encontrar con más actualizaciones o las mismas que **master**, pero **master**  nunca debe de tener más actualizaciones que **develop**. Por esta razón cada vez que se hace una actualización o fix a un feature es necesario crear una rama a partir de **develop** pues es la rama más completa siempre.

[Subir](#top)

<a name="item4"></a>
### D) Instalación e inicialización
---
Primero instala las siguientes dependencias el la raiz del proyecto con el siguiente comando:

```properties
npm install
```

Si modificas el Tailwind, corre el siguiente comando

```properties
npx tailwindcss-cli@latest build src/styles/index.css -o src/styles/tailwind.css
```

Para visualizar el proyecto en tu directorio, corre el siguiente comando:

```properties
npm start
```

Los iconos se obtiene de la siguiente página de la sección Material Design:

    https://react-icons.github.io/react-icons/
    
Para componentes como los modales se usan las siguientes librerias:

```
https://reactstrap.github.io/

https://react-bootstrap.github.io/ 
```


Corre la aplicacion en modo desarrollo\
Abre [http://localhost:3000](http://localhost:3000) para visualizarla en tu navegador.

La pagina se recargara si relizas cambios.\
También verá cualquier error de lint en la consola.

[Subir](#top)

<a name="item5"></a>
### E) Vista general de las carpetas y archivos
---

```
vectordigital-web/
    node_modules/
    public/
        index.html
        favicon.ico
    src/
        actions/
        assets/
        components/
        constants/
        Context/
        customHooks/
        docs/
        mocks/
        reducers/
        sagas/
        store/
        styles/
        types/
        utils/
        App.tsx
        index.tsx
    .env
    .eslintcache
    .gitignore
    craco.config.js
    package-lock.json
    package.json
    postcss.config.js
    README.md
    tailwind.config.js
    tsconfig.json
```
[Subir](#top)

<a name="item6"></a>
### F) Definición de las carpetas y archivos de Redux-Saga
---

Como se menciono anteriormente el proyecto se realizó con la librería de [Redux](https://redux.js.org/ "Sitio oficial de Redux.") y [Redux-Saga](https://redux-saga.js.org/ "Sitio oficial de Redux-Saga.") para manejar los estados de la aplicación y para esto se necesita de **actions**, **reducers** y **sagas**, estas funciones permiten la llamada y la actualización de los datos a través de una **store**.

El **store** es donde está el estado de toda la aplicación, y lo accedemos a través de los **actions** y **reducers**. 

Dentro del proyecto se encontrarán las siguientes carpetas:

* **types/**

Como en este proyecto se está utilizando [TypeScript](https://www.typescriptlang.org/ "Sitio oficial de TypeScript.") , es importante declarar los tipos que va a necesitar el api que se quiera conectar. Aparte de lo que recibe la api, los types deben de tener un objeto que describa su estado mediante una interfaz. 

Todos los objetos de estado cuentan con un atributo loading, el cual describe si se le está hablando a la api. También tiene un atributo error, si la api llegará a fallar, aquí se guardaría el error. Así mismo,  contiene el atributo message donde se guarda el endpoint de la api a la que se va a hablar. Finalmente el objeto state contiene de atributos lo que recibe de la api o los parámetros que se le tiene que mandar. 

_NOTA:_ A continuacion los codigos de ejemplo estarán basados en objetos de ***listas*** pero eso aplica al estado de cualquier componente que requiera su aplicación.

```ts
export interface ListState {
    loading: boolean;
    list: IList[];
    error: string | null;
    message: string;
    id: any;
}
```
* **actions/**

En esta carpeta se encuentran las **actions** de las **sagas**. Las **actions**, como su nombre lo dice, son acciones que se envían a la **store** para pedir información. Para este proyecto se utilizan tres tipos de **actions**: `REQUEST`, `RECEIVE`, `ERROR`. 

`REQUEST` es el **action** que manda a llamar a la api. Modifica el estado loading para decir que le está llamando a la api.

`RECEIVE` es el **action** que recibe los datos de la api. Modifica el estado de los datos de la api y cambia el loading a false cuando ya recibió la información de la api.

`ERROR` es el **action** que se modifica si llega a haber un error con la api. Modifica los estados de error y de loading, indicando que ya se le habló a la api. 

La estructura de un componente **action** es de la siguiente manera:

```ts
import { LIST_API_REQUEST, LIST_API_RECEIVE, LIST_API_ERROR, LIST_API_RESET } from './actionTypes';
import { GetListRequest, GetReceiveList, GetListError, GetListRequestPayload, GetListReceivePayload, GetListErrorPayload, GetListReset, GetListResetPayload } from '../types/ListTypes';

export const getListRequest = (payload: GetListRequestPayload): GetListRequest => ({
    type: LIST_API_REQUEST,
    payload,    
});

export const getListRecieve = ( payload: GetListReceivePayload ): GetReceiveList => ({
    type: LIST_API_RECEIVE,
    payload,
});

export const getListError = ( payload: GetListErrorPayload ): GetListError => ({
    type: LIST_API_ERROR,
    payload,
}); 


export const getListReset = ( payload: GetListResetPayload ): GetListReset => ({
    type: LIST_API_RESET,
    payload,
});
```
- En el primer import se encuentra las variables de las actions, estas se declaran en el component de actionTypes. 
- En el segundo import se encuentra los types que se declararon en la carpeta de types.

El resto del componente contiene las **actions** de `request`, `receive` y de `error`. Cada una de estas debe de ser una constante que se exporta, y que, además se debe de establecer su type. Recibe de parámetro el payload y debe de contener el type que se estableció en la carpeta de types. Dentro de esta constante se debe de poner el type, el cual se saca de la carpeta de actionTypes, y el payload. 

En el componente de actionTypes se tiene que declarar una variable con las **actions** de la siguiente manera:

```ts
//Action types

export const LIST_API_REQUEST = 'LIST_API_REQUEST';
export const LIST_API_RECEIVE = 'LIST_API_RECEIVE';
export const LIST_API_ERROR = 'LIST_API_ERROR';
export const LIST_API_RESET = 'LIST_API_RESET';
```

de esta manera se establece los actions que debe de tener una llamada a api.

* **reducers/**

En esta carpeta se encuentran los reducers. Estos definen como cambian los estados del proyecto de acuerdo a las actions. La estructura del componente reducer es de la siguiente manera:

```ts
import * as actionTypes from '../actions/actionTypes';
import { ListActions, ListState } from '../types/ListTypes';

const initialState: ListState = {
    loading: true,
    list: [{
        list_name: "",
        list_id: "",
        list_size: "",
        vector: false,
        ultima: false,
        emisoras: []
    }],
    error: null,
    message: "",
    id: 0,
};

const listReducer = ( state = initialState, action: ListActions ) => {
    switch(action.type){
        case actionTypes.LIST_API_REQUEST:
            return { ...state, loading: true, message: action.payload.message, id: action.payload.id };
        
        case actionTypes.LIST_API_RECEIVE:
            return {...state, loading: false, list: action.payload.list, error: null};

        case actionTypes.LIST_API_ERROR:
            return { ...state, loading: false, list: initialState.list, error: action.payload.error };

        case actionTypes.LIST_API_RESET:
            //console.log("reset fondosCpaEmisReducer");
            return { ...initialState };
        
        default: 
            return { ...state };
    }
}

export default listReducer;
```
Se debe de importar actionTypes también se debe de importar los types del estado y las actions  de la api. 

Es importante siempre de tener un estado inicial. Debido a esto se debe de tener un objeto con el type del state de la api, e inicializar los estados con un valor. 

También se debe de tener una constante que se va a exportar y que es la estructura del reducer en sí. 

Recibe de parámetros el estado inicial, y los actions del type. 
Dentro de la constante debe de haber un switch para poder saber que tipo de action es la que se está ejecutando. En cada caso debe de establecerse el tipo de action que se busca, y modificar  sólo los estados que se están actualizando. Por default se regresa el state sin mutar.

Después de crear un reducer se debe de importar en el rootReducer.tsx, de esta manera será accesible en los demás componentes, como se muestra en el siguiente ejemplo:

```ts
import { combineReducers } from 'redux';
import listReducer from './listReducer';


const rootReducer = combineReducers({
    list: listReducer,
    ...,
});

export type RootState = ReturnType<typeof rootReducer>;

export default rootReducer;
```

* **sagas/**

La saga es el middleware que logra conectar redux con la api. Se encarga de llamarle a la api y de mandar lo recibido a la action correspondiente. La estructura es la siguiente:

```ts
import axios from 'axios';
import { all, call, put, SagaReturnType, takeLatest } from 'redux-saga/effects';

import { getListError, getListRecieve } from '../actions/listAction';
import { LIST_API_REQUEST } from '../actions/actionTypes';
import { IList } from '../types/ListTypes';
import * as apiCall from '../constants';

type ListUser = SagaReturnType<typeof getList>

const getList = (data: string, params: any) =>
    axios.get<IList[]>(apiCall.LISTAS_API+data, params);

function* getListSaga(action: any) {
    
    let config = {
        headers: {
            'x-api-key': apiCall.X_API_KEY_LISTAS,
            'id': action.payload.id,
        },
    }

    try{
        
        const listUser: ListUser = yield call(getList, action.payload.message, config);

        yield put( getListRecieve({ list: listUser.data }) );

    } catch (e:any) {

        yield put( getListError({ error: e.message }));

    }
}

function* listSaga() {

    yield all([takeLatest(LIST_API_REQUEST, getListSaga)]);

}

export default listSaga;
```
Para las solicitudes http que realizará la saga se debe de importar la librería de axios, y algunos componentes de la misma librería de saga. También, del componente de actions se necesita el action de receive y de error que es donde se van a mandar los datos recibidos, así mismo, se necesita el actionType del request de esa api y los types de lo que se va a recibir de la api. Ya por último, se debe de importar las constantes donde se encuentran los secret.

Para hacer el get de la api se necesita definir su type, para lograr esto se debe de poner en el typeof el nombre de la función donde se hace llamada. Una vez hecho eso ya se puede hacer la función donde se hace el get. Al hacer el get se debe de poner el tipo de data que es el que recibe, de no saber lo que se recibe se puede poner any. De parámetros recibe el secret, el endpoint y los parámetros.  

Una vez hecho esto ya se puede hacer la función generadora, esta recibe de parámetros el action payload de tipo any. Dentro de esta función se establecen los parámetros necesarios para la consulta de la api como lo son los headers o alguna otra configuración de ser necesaria, el resto de código será encerrado en un try-catch. En el try se hace la llamada a la función del get, dentro de un yield hay un call, este contiene la función donde hace la llamada al payload donde se encuentra el endpoint y los parámetros. Los datos recibidos de esa llamada se guardan dentro del action receive. Si llegará a haber un problema con la api se va al catch, donde los datos recibidos se envían al action de error.

Finalmente, se hace otra función generadora, aquí se le va a hablar a la función donde se hace la llamada a la api y se le asigna el type de request, indicando que si se hace request, estos son los pasos que se van a disparar. Como nota importante, el nombre de esta función debe tener el mismo nombre que el componente y es el que se exporta. 

Una vez hecho esto, este componente se tiene que importar en el rootSaga.tsx, para poder ser utilizado por todos los componentes. 

```ts
import { all, fork } from 'redux-saga/effects';
import listSaga from './listSaga';

export function* rootSaga() {

    yield all(
        [
            fork(listSaga),
            ...,
        ]
    );
}
```
* **store/** 

Se declara el middleware de redux-saga.

* **assets/**

Logos e íconos que no son de una librería.

* **components/**
  
Se crea el layout de las páginas. Cada página contiene más componentes pequeños adentro. Ejemplos: Portafolio, Trading, Fondos, etc.

* **constants/**
  
Son los archivos con las constantes del proyecto. Index es donde se declaran las variables de ambiente.

* **containers/**
  
Son componentes reutilizables que se utilizan en las pantallas. Ejemplos: Operaciones, Noticias, Movers, etc.

* **mocks/**
  
Se utilizan para declarar la configuración y/o componentes que se mandan a otro componente.

* **styles/**
  
Archivos css creados por nosotros, en su mayoría son para los containers de tablas, y por tailwind.

* **utils/**

Métodos que se utilizan para limpiar código o formatear variables

[Subir](#top)

