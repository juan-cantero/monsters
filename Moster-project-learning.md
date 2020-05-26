# React

### Que es react y que resuelve
React es una libreria que nos permite programar en forma mas declarativa, en lugar de escribir codico como Document.querySelector(".class") para buscar elemento por elemento en el  DOM y luego agregarle un listener con una funcion etc, con react simplemente podemos decir que propiedades queremos cambiar y react (reacciona a eso ) y se encarga de cambiar el DOM por nosotros.

### El trabajo de un desarrollador react
1. Decidir sobre los componentes
2. Decidir sobre el estado de los mismos
3. Que cambia cuando el estado cambia

### Comenzar:
***para instalar react***

npx create-react-app my-app
cd my-app
npm start

npx permite ejecutar comandos de paquetes npm que no tenemos instalado localmente
npm start inicializa el servidor.
"cuando corremos **npx create-react-app** react va a instalar un monton de dependencias tales como babel, webpack, jest. Ademas va a dejar nuestro proyecto con una estructura

### Componentes

Los componentes nos permiten separar nuestro UI en pequeñas piezas como si se tratase de un lego 

```jsx
class App extends Component {
  constructor() {
    super();
    this.state = {
      monsters : []
    }
  }

  componentDidMount() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response => response.json())
      .then(users => this.setState({monsters:users}))
      
  }

  render() {
    return (
      <div className="App">
        <CardList monsters={this.state.monsters}/>

      </div>
    )
  }
}


const CardList = props  => {
    return (
        // props.monsters viene de  <CardList monsters={this.state.monsters}/>
        // props hace referencia a lo que pasemos en nuestra instancia 
        //<CardList prop1={algo} prop2={algo}> y para acceder usamos prop.prop1
        // prop.prop2 etc.
        <div className="card-list"> {props.monsters.map(monster => 
          <Card key={monster.id} monster={monster}/>)
          }
        </div>
    )
}


export const Card = (props) => {
    return (
        // props.monster viene de CardList, cada mounstruo es mapeade en una nueva instancia
        // de <Card/>
        <h1> {props.monster.name}</h1>
    )
}


```

pueden ser tanto funciones como clases(que en definitiva son funciones en javascript).
React use jsx que se parece a html pero en verdad es javascript.


## Donde poner el estado

Ejemplo:
```javascript
  //Part of SearchBox
   <input
        className="search"
        type="search"
        placeholder={placeHolder}
        onChange={handleChange}
    >
    </input>
    
    //Part of App
    <SearchBox
          placeHolder="search monsters"
          handleChange = {e => this.setState({searchField: e.target.value})} 
        />
```
En este caso ponemos el estado en App porque lo necesitamos para el componente lista tambien
cuando un usuario ingresa lo que quiere buscar en el input de SearchBox este llama el metodo 
habldeChange que esta definido en App (se llama lifting-up state).
El flujo de datos en react es unidireccional.


## Bind y el scope de this 

```javascript
class Person {
  constructor(name) {
    this.name = name
    //this.sayName = this.sayName.bind(this)
  }

  sayName() {
    return this.name;
  }

  sayName2= () => {
    return this.name;
  }
}

const p = new Person("juan");
var sayName = p.sayName;
var sayName2 = p.sayName2;

console.log(sayName()); // undefined
console.log(sayName2()); // juan

}

```
el metodo definido con arroy function devuelve juan mientras que el el metodo definido
convencionalmente no, esto es poque las funciones en javascript usan su propio scope, 
entonces cuando creamos sayName = p.sayName la funcion tendro otro objecto como su nuevo 
scope, mientras que las funciones con arrow usan el scope de donde fueron definidas en lugar
de crear su  propio scope (hacen un lookup).

La otra forma de usar la funcion sin arrow es usando bind.
