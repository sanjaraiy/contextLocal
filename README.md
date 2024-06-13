# Todo Application Using React
This project is built on `JavaScript`, `Tailwind`, `React` and `React-Redux`.It is responsive for different devices.

## Install Vite :-
```
vite@latest
```
## Install Node-Modules :-
```
npm install
```
## Tailwind Install Link :-
```
npm install -D tailwindcss
npx tailwindcss init
```
## Tailwind Configure your template paths :-
```
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
## Add the Tailwind directives to your CSS :-
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```
## Install React-Redux :-
```
npm i react-redux
```

## Features :-
- Add new todo.
- Update existing todo.
- Save todo in local Storage.
- Delete todo from a list.
- Toggle an existing todo.

## Contexts :-
### Index.js :-
```js
import {TodoContext, Todoprovider, useTodo } from './TodoContext'

export {
    TodoContext,
    Todoprovider,
    useTodo
}

```

### TodoContext.js :-
```js
import {createContext, useContext} from "react";

export const TodoContext = createContext({
     todos : [
        {
            id : 1,
            todo : "Todo msg",
            completed : false,
        },
     ],
     addTodo : (todo) => {},
     updatedTodo : (id, todo) => {},
     deleteTodo : (id) => {},
     toggleComplete : (id) => {}
})

export const useTodo = () => {
    return useContext(TodoContext)
}

export const Todoprovider = TodoContext.Provider

```

## App.js :-
```js

import { useState } from 'react'
import { Todoprovider } from './contexts'
import { useEffect } from 'react'

function App() {
  
  const [todos, setTodos] = useState([])

  const addTodo = (todo) => {
       setTodos((prev) => [{id: Date.now(),...todo}, ...prev])
  }

  const updatedTodo = (id, todo) => {
       setTodos((prev) => prev.map((prevTodo) => (prevTodo.id === id ? todo : prevTodo)))

  }

  const deleteTodo = (id) => {
      setTodos((prev) => prev.filter((todo) => todo.id !== id))
  }

  const toggleComplete = (id) => {
      setTodos((prev) => prev.map((prevTodo) => prevTodo === id ? { ...prevTodo, completed : !prevTodo.completed} : prevTodo))
  }

  useEffect(() => {
   const todos = JSON.parse(localStorage.getItem("todos"))
   if(todos && todos.length > 0){
      setTodos(todos)
   }
  },[])

  useEffect( () => {
    localStorage.setItem("todos", JSON.stringify(todos))

  }, [todos])
  
  return (
    <Todoprovider value = {{todos, addTodo, updatedTodo, deleteTodo, toggleComplete}}>
       <div className = "bg-[#172842] min-h-screen py-8">
       <div className = "w-full max-w-2xl mx-auto shadow-md rounded-lg px-4 py-3 text-white">
        <h1 className = "text-2xl font-bold text-center mb-8 mt-2">Manage Your Todos</h1>
        <div className = "mb-4">
            {/* Todo form goes here */} 
        </div>
        <div className = "flex flex-wrap gap-y-3">
            {/*Loop and Add TodoItem here */}
        </div>
    </div>
   </div>
    </Todoprovider>
   
  )
}

export default App

```

