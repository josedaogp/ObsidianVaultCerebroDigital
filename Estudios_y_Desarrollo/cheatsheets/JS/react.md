# ⚛️ Cheatsheet de React

Este cheatsheet cubre los aspectos esenciales de React, la popular biblioteca de JavaScript para construir interfaces de usuario.

## 🔍 Conceptos Básicos

*   **Componente (Component):** 🧱 Bloque de construcción fundamental de una aplicación React.  Representa una parte de la UI (interfaz de usuario).  Puede ser una función (componente funcional) o una clase (componente de clase).
*   **JSX (JavaScript XML):** 📝 Sintaxis similar a HTML que se usa para describir la estructura de la UI en React.  Se transpila a JavaScript puro.
*   **Props (Properties):** 📥 Datos que se pasan de un componente padre a un componente hijo.  Son *de solo lectura* dentro del componente hijo.
*   **State (Estado):** 💾 Datos internos de un componente que pueden cambiar a lo largo del tiempo.  Cuando el estado cambia, React vuelve a renderizar el componente.
*   **Virtual DOM:** 🌳 Representación en memoria del DOM real.  React usa el Virtual DOM para optimizar las actualizaciones de la UI.
*   **Hook:** 🎣 Función especial que permite "engancharse" al estado y ciclo de vida de React en componentes funcionales (antes solo disponibles en componentes de clase).
*   **Unidirectional Data Flow:** ➡️ Los datos fluyen en una sola dirección (de padres a hijos).

## 📦 Instalación y Configuración

1.  **Create React App (recomendado):**

    ```bash
    npx create-react-app mi-app  # Crea un nuevo proyecto
    cd mi-app
    npm start  # Inicia el servidor de desarrollo
    ```
    *   `create-react-app` configura todo lo necesario (Webpack, Babel, etc.) para empezar a desarrollar con React.

2.  **Configuración manual (menos común):**

    *   Necesitarás instalar `react`, `react-dom`, y un bundler (como Webpack) y un transpilador (como Babel).

## 🚀 Componentes

1.  **Componentes Funcionales (Functional Components) - Recomendado:**

    ```javascript
    import React from 'react';

    function MiComponente(props) {
      return (
        <div>
          <h1>Hola, {props.nombre}!</h1>
        </div>
      );
    }

    export default MiComponente;
    ```
    *   Son funciones de JavaScript que reciben `props` como argumento y devuelven un elemento de React (JSX).
    *   Se recomienda usar componentes funcionales con *hooks* para la mayoría de los casos.

2.  **Componentes de Clase (Class Components) - Menos común hoy en día:**

    ```javascript
    import React, { Component } from 'react';

    class MiComponente extends Component {
      render() {
        return (
          <div>
            <h1>Hola, {this.props.nombre}!</h1>
          </div>
        );
      }
    }

    export default MiComponente;
    ```
    *   Son clases de JavaScript que extienden `React.Component`.
    *   Tienen un método `render()` que devuelve el elemento de React.
    *   El estado se maneja con `this.state` y se actualiza con `this.setState()`.

## 📝 JSX

*   Es una extensión de JavaScript que permite escribir código similar a HTML dentro de JavaScript.
*   Se transpila a llamadas a `React.createElement()`.
*   Debe haber un elemento raíz (ej. `<div>`, `<>`, `<React.Fragment>`).
*   Las expresiones de JavaScript se escriben entre llaves `{}`.
*   Los atributos se escriben en camelCase (ej. `className`, `onClick`, `onChange`).
*   Los comentarios en JSX se escriben entre llaves: `{/* Comentario */}`

```javascript
function MiComponente() {
  const nombre = "Juan";
  return (
    <> {/* Fragmento (elemento raíz sin nodo DOM adicional) */}
      <h1>Hola, {nombre}!</h1>  {/* Expresión de JavaScript */}
      <p className="mi-clase">Este es un párrafo.</p>  {/* Atributo en camelCase */}
      <button onClick={() => alert("¡Clic!")}>Haz clic</button>  {/* Evento */}
    </>
  );
}
```

## 📥 Props

*   Son la forma de pasar datos de un componente padre a un componente hijo.
*   Son de solo lectura dentro del componente hijo (inmutables).

```javascript
// Componente padre
function App() {
  return (
    <MiComponente nombre="Ana" edad={30} />
  );
}

// Componente hijo (funcional)
function MiComponente(props) {
  return (
    <div>
      <p>Nombre: {props.nombre}</p>
      <p>Edad: {props.edad}</p>
    </div>
  );
}

// Componente hijo (clase)
class MiComponente extends React.Component {
    render(){
        return(
            <div>
              <p>Nombre: {this.props.nombre}</p>
              <p>Edad: {this.props.edad}</p>
            </div>
        )
    }
}
```

* **PropTypes**: Para validar el tipado de las props.

## 💾 State (Estado)

*   El estado es un objeto que contiene datos internos de un componente.
*   Cuando el estado cambia, React vuelve a renderizar el componente.

1.  **En componentes de clase:**

    *   Se inicializa en el constructor.
    *   Se accede con `this.state`.
    *   Se actualiza con `this.setState()`.  *Nunca* modifiques `this.state` directamente.

    ```javascript
    class Contador extends React.Component {
      constructor(props) {
        super(props);
        this.state = { contador: 0 };
      }

      incrementar() {
        this.setState({ contador: this.state.contador + 1 });
      }
      // También se puede pasar una función a setState
      //this.setState((prevState, props) => ({contador: prevState.contador + 1}))

      render() {
        return (
          <div>
            <p>Contador: {this.state.contador}</p>
            <button onClick={() => this.incrementar()}>Incrementar</button>
          </div>
        );
      }
    }
    ```

2.  **En componentes funcionales (con el hook `useState`):**

    ```javascript
    import React, { useState } from 'react';

    function Contador() {
      const [contador, setContador] = useState(0);  // Valor inicial: 0

      return (
        <div>
          <p>Contador: {contador}</p>
          <button onClick={() => setContador(contador + 1)}>Incrementar</button>
        </div>
      );
    }
    ```
    *   `useState` devuelve un array con dos elementos: el valor actual del estado y una función para actualizarlo.

## 🎣 Hooks

Los hooks son funciones que permiten "engancharse" a características de React (como el estado y el ciclo de vida) desde componentes funcionales.

1.  **`useState`:**  Para manejar el estado.

2.  **`useEffect`:**  Para realizar efectos secundarios (side effects) como peticiones a APIs, suscripciones, manipulación directa del DOM, etc.

    ```javascript
    import React, { useState, useEffect } from 'react';

    function MiComponente() {
      const [datos, setDatos] = useState(null);

      useEffect(() => {
        // Se ejecuta después de cada renderizado (por defecto)
        fetch('https://api.example.com/datos')
          .then(response => response.json())
          .then(data => setDatos(data));

        // Función de limpieza (opcional) - se ejecuta antes de que el componente se desmonte o antes del siguiente useEffect
        return () => {
          // Limpiar suscripciones, temporizadores, etc.
        };
      }, []); // Array de dependencias vacío:  se ejecuta solo una vez (componentDidMount)

      return (
        <div>
          {datos ? <pre>{JSON.stringify(datos, null, 2)}</pre> : <p>Cargando...</p>}
        </div>
      );
    }
    ```
    *   `useEffect` recibe dos argumentos: una función (el efecto) y un array de dependencias (opcional).
    *   Si el array de dependencias está vacío (`[]`), el efecto se ejecuta solo una vez (después del primer renderizado), similar a `componentDidMount` en componentes de clase.
    *   Si el array de dependencias contiene variables, el efecto se ejecuta cada vez que alguna de esas variables cambie.
    *   La función de limpieza (opcional) se ejecuta antes de que el componente se desmonte o antes de que el efecto se vuelva a ejecutar.

3.  **`useContext`:**  Para acceder a un contexto de React.

4.  **`useReducer`:**  Alternativa a `useState` para manejar estados complejos (similar a Redux).

5.  **`useCallback`:**  Devuelve una versión memorizada de una función (útil para optimizar el rendimiento en componentes hijos que dependen de esa función).

6.  **`useMemo`:**  Devuelve un valor memorizado (útil para evitar cálculos costosos en cada renderizado).

7.  **`useRef`:**  Devuelve un objeto ref mutable.  Se usa comúnmente para acceder a elementos del DOM directamente, o para guardar valores que persisten entre renderizados sin causar un nuevo renderizado.

8.  **`useLayoutEffect`**: Similar a useEffect, pero se ejecuta sincrónicamente después de todas las mutaciones del DOM.

9. **`useImperativeHandle`:** Personaliza el valor de la referencia que se expone a componentes padres al usar ref.

10. **`useDebugValue`**: Muestra una etiqueta para custom hooks en las React DevTools.

*   **Reglas de los Hooks:**
    *   Solo se pueden llamar en el nivel superior de un componente funcional o de otro hook.
    *   No se pueden llamar dentro de bucles, condicionales o funciones anidadas.

## ⚡️ Eventos

*   Los eventos en React se nombran en camelCase (ej. `onClick`, `onChange`, `onSubmit`).
*   Se pasan como funciones (no como cadenas).

```javascript
function MiFormulario() {
  const [valor, setValor] = useState('');

  const handleChange = (event) => {
    setValor(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault(); // Evitar el comportamiento predeterminado del formulario
    alert('Valor enviado: ' + valor);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={valor} onChange={handleChange} />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

## 🔄 Ciclo de Vida (Componentes de Clase)

*   **`componentDidMount`:**  Se ejecuta después de que el componente se monta (se inserta en el DOM).  Ideal para hacer peticiones a APIs, suscripciones, etc.
*   **`componentDidUpdate`:**  Se ejecuta después de que el componente se actualiza (recibe nuevas props o cambia el estado).
*   **`componentWillUnmount`:**  Se ejecuta justo antes de que el componente se desmonte (se elimine del DOM).  Ideal para limpiar suscripciones, temporizadores, etc.
*   **`shouldComponentUpdate`:** Permite controlar si el componente debe actualizarse o no.  Útil para optimización.
*   **`getDerivedStateFromProps`**: Se invoca antes de render, y sirve para actualizar el state en base a cambios en las props.
*   **`getSnapshotBeforeUpdate`**: Se invoca antes de que los cambios del virtual DOM se apliquen al DOM real.

*Nota:* En componentes funcionales, `useEffect` reemplaza a `componentDidMount`, `componentDidUpdate` y `componentWillUnmount`.

## 🔑 Listas y Keys

*   Cuando se renderiza una lista de elementos, cada elemento debe tener un atributo `key` único.
*   `key` ayuda a React a identificar qué elementos han cambiado, se han agregado o se han eliminado, y optimiza el rendimiento.
*   La `key` debe ser un valor único y estable entre renderizados (no uses el índice del array como `key` si el orden de los elementos puede cambiar).

```javascript
function ListaDeFrutas(props) {
  return (
    <ul>
      {props.frutas.map(fruta => (
        <li key={fruta.id}>{fruta.nombre}</li>
      ))}
    </ul>
  );
}
```

## 📝 Formularios

1.  **Componentes controlados (Controlled Components):**

    *   El estado del formulario se controla completamente por React.
    *   El valor de los inputs se enlaza al estado con el atributo `value`.
    *   Los cambios se manejan con el evento `onChange`.

    ```javascript
     function FormularioControlado() {
        const [valor, setValor] = useState('');

        const handleChange = (event) => {
          setValor(event.target.value);
        };

        return (
          <input type="text" value={valor} onChange={handleChange} />
        );
     }
    ```

2.  **Componentes no controlados (Uncontrolled Components):**

    *   El estado del formulario se maneja por el DOM.
    *   Se usa `useRef` para acceder a los elementos del DOM directamente.

## 🚀 Renderizado Condicional

```javascript
function Saludo(props) {
  if (props.estaLogueado) {
    return <h1>Bienvenido!</h1>;
  } else {
    return <h1>Por favor, inicia sesión.</h1>;
  }
}

// Operador ternario
function Mensaje(props){
    return(
        <div>
            {props.tieneMensajes ? <p>Tienes mensajes</p> : <p>No tienes mensajes.</p>}
        </div>
    )
}

// Operador lógico &&
function Notificaciones(props){
 return(
    <div>
        {props.cantidad > 0 && <p>Tienes {props.cantidad} notificaciones</p>}
    </div>
 )
}
```

## 🌐 Context API

* Permite compartir datos entre componentes sin tener que pasarlos explícitamente a través de props en cada nivel del árbol de componentes.
* Útil para datos globales como el tema actual, el usuario autenticado, etc.
* Se compone de un `Provider` (proveedor) y un `Consumer` (consumidor).

```javascript
// 1. Crear el contexto
const MiContexto = React.createContext(valorInicial);

// 2. Proveer el contexto
function App() {
  return (
    <MiContexto.Provider value={/* valor del contexto */}>
      {/* Componentes que pueden acceder al contexto */}
    </MiContexto.Provider>
  );
}

// 3. Consumir el contexto (en un componente funcional)
function MiComponente() {
  const valor = useContext(MiContexto);
  return <p>{valor}</p>;
}

// 3. Consumir el contexto (en un componente de clase).
class MyComponent extends React.Component {
  static contextType = MiContexto; //Forma 1
  render() {
    let value = this.context;
    /* renderiza algo aquí */
  }
}

//Forma 2 para consumir, con el Consumer
<MiContexto.Consumer>
    {value => /* muestra algo basado en el valor del contexto */}
</MiContexto.Consumer>
```

## ⬆️ Composición vs Herencia

* React recomienda usar *composición* en lugar de herencia para reutilizar código entre componentes.
* La composición se basa en pasar componentes como props (children) a otros componentes.

## ⚛️ React Router (para navegación)

No es parte de React, pero es la biblioteca más popular para manejar la navegación en aplicaciones React.

```bash
npm install react-router-dom
```

```javascript
import { BrowserRouter as Router, Route, Routes, Link } from 'react-router-dom';

function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li><Link to="/">Inicio</Link></li>
            <li><Link to="/acerca">Acerca de</Link></li>
          </ul>
        </nav>

        <Routes>
          <Route path="/" element={<Inicio />} />
          <Route path="/acerca" element={<AcercaDe />} />
        </Routes>
      </div>
    </Router>
  );
}
```

## ✨ Otros Conceptos

*   **Fragmentos (`<React.Fragment>` o `<> </>`):** Permiten agrupar elementos sin agregar un nodo extra al DOM.
*   **Portales (`ReactDOM.createPortal`):** Permiten renderizar un componente en un nodo del DOM que está fuera de la jerarquía del componente actual (útil para modales, tooltips, etc.).
*   **Refs:** Proporcionan una forma de acceder a nodos del DOM o a instancias de componentes de clase desde componentes funcionales.
*   **Error Boundaries:** Componentes que capturan errores de JavaScript en cualquier parte de su árbol de componentes hijo, registran esos errores y muestran una UI de respaldo.
*   **Higher-Order Components (HOCs):** Funciones que toman un componente y devuelven un nuevo componente mejorado (patrón avanzado).
* **Render Props:** Una técnica para compartir código entre componentes de React utilizando una prop cuyo valor es una función.
* **StrictMode**: Componente para resaltar problemas potenciales.

Este cheatsheet cubre los fundamentos de React y muchos conceptos avanzados. ¡Guárdalo y consúltalo a menudo!