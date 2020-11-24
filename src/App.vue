<template>
  <div class="burden">
    <h1>Vue 3 Todo App</h1>
    <form @submit.prevent="addNewTodo">
      <input v-model="newTodo" name="newTodo" placeholder="Rule the world.." maxlength="45">
    </form>
    
    <div class="buttonLayout">
      <button @click="markAllDone" class="pointer">Mark All Done</button>
      <button @click="removeAllTodos" class="pointer">Remove Finished</button>
    </div>
    <ul>
      <transition-group name="fading" tag="ul">
      <li v-for="(todo, index) in todos" :key="todo.id">
        <h3 :class="{ done: todo.done }" @click="toggleDone(todo)" class="todo">
          <span class="line-through">
            {{todo.content}}
          </span>
        </h3>
        <button @click="removeTodo(index)"><i class="fas fa-trash"></i></button>
      </li>
      </transition-group>
    </ul>

  </div>
</template>

<script>
import { ref } from 'vue';

export default {

  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos);
    }
  },

  watch: {
    todos: {
      handler(newTodo) {
        localStorage.todos = JSON.stringify(newTodo);
      },
      deep: true
    }
  },

  setup() {
    const newTodo = ref('');
    const todos = ref([]);

    function addNewTodo() {
      if (newTodo.value == '') {
        alert('Enter in a task!')

      } else if (todos.value.length >= 7) {
        alert('Check some stuff off your current list.');

        } else {
        todos.value.push({
          id: Date.now(),
          done: false,
          content: newTodo.value,
        });
        newTodo.value = '';
      }
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
    }

    function markAllDone() {
      todos.value.forEach((todo) => todo.done = true);
    }

    function removeAllTodos() {
      todos.value = todos.value.filter(todo => !todo.done);
    }

    return {
      todos,
      newTodo,
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAllTodos,
    };
  }
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100&display=swap");

* {
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Montserrat', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(to top right, rgba(37,93,161,1) 0%, rgba(20,138,33,1) 100%);
  min-height: 600px;
}

#app {
  justify-content: center;
  width: 300px;
  min-height: 400px;
}

.burden {
  display: flex;
  flex-direction: column;

  h1 {
    color: #fff;
    margin-bottom: 10px;
    text-align: center;
  }
  input {
    width: 100%;
    border: none;
    background: none;
    border-bottom: 1px solid #fff;
    margin-bottom: 20px;
    text-align: center;
    color: #fff;
    font-size: 1.5em;
  }
  ::placeholder {
    color: #fff;
  }
  input:focus {
    outline: none;
  }
  button {
    font-size: 1em;
  }

  .pointer {
    cursor: pointer;
    transition: .5s;
  }
  .pointer:hover {
    background-color: rgba(255, 255, 255, 0.2);
  }
  .pointer:active {
    background-color: rgba(51, 51, 51, 0.2);
  }
}

.todo {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
  text-decoration-thickness: 1px;
  color: #fff;
}
.fading-enter-active, .fading-leave-active {
  transition: opacity ease .25s, transform ease-in-out 0.5s;
  transform: translateX(-50%);
}
/* .todo-item-leave-active below version 2.1.8 */
.fading-enter, .fading-leave-to {
  opacity: 0;
  transform: translateX(50%);
}
li {
  color: #fff;
  display: flex;
  justify-content: space-between;
  list-style-type: none;
  margin-bottom: 20px;
  font-size: 18px;
  transition: all .5s;
  button {
    color: #fff;
    background: none;
    border: none;
    cursor: pointer;
    transition: .5s;
  }
  button:hover {
    color: #ff8a8a;
  }
}

.buttonLayout{
  display: flex;
  justify-content: space-evenly;
  margin-bottom: 20px;
  button {
    color: #fff;
    background: none;
    border: 1px solid #fff;
    padding: 5px 10px;
  }
}

</style>
