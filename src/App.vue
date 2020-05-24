<template>
  <div id="app">
    <h1 class="text-center mt-5">Shopping List App</h1>
    <div class="container mt-5">
      <b-form @submit.prevent="addItem">
        <b-form-group label="Enter Your Item">
          <b-form-input 
            type="text" 
            placeholder="Type Here" 
            v-model="inputValue">
          </b-form-input>
        </b-form-group>
      </b-form>
      <div class="mt-3 mb-5">
        <b-button class="mr-1" variant="primary" @click="addItem">Add</b-button>
        <b-button class="ml-1" variant="danger" @click="deleteAll">Delete All</b-button>
      </div>
      <b-list-group>
        <b-list-group-item 
          class="d-flex justify-content-between align-items-center" 
          v-for="(todo, index) in todos" 
          :key="todo.id">
          <div>
            <b-form-checkbox v-model="todo.completed">
              <span :class="{strike: todo.completed}">
                <input class="border-0" :class="{'input': !todo.disabled}" type="text" v-model="todo.text" :disabled="todo.disabled">
              </span>
            </b-form-checkbox>
          </div>
          <div>
            <b-icon class="mr-2" icon="pencil-square" variant="warning" @click="editItem(index)"></b-icon>
            <b-icon icon="x-square" variant="danger" @click="deleteItem(index)"></b-icon>
          </div>
        </b-list-group-item>
      </b-list-group>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        inputValue: '',
        todos: [],
      }
    },
    methods: {
      clearInput() {
        this.inputValue = ''
      },
      addItem() {
        if (this.inputValue) {
          this.todos.push(
            {id: this.todos.length + 1, text: this.inputValue, completed: false, disabled: true}
          )
          this.clearInput()
        } 
      },
      deleteItem(index) {
        this.todos.splice(index, 1)
        this.clearInput()
      },
      deleteAll() {
        this.todos = []
        this.clearInput()
      },
      editItem(index) {
        if (!this.todos[index].completed)
        this.todos[index].disabled = !this.todos[index].disabled
      },
    },
    watch: {
      todos: {
        handler() {
          localStorage.setItem('todos', JSON.stringify(this.todos))
        },
        deep: true
      }
    },
    mounted() {
      if (localStorage.getItem('todos')) {
        this.todos = JSON.parse(localStorage.getItem('todos'));
      }
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .strike {
    text-decoration: line-through;
    opacity: 0.3;
  }
  .input {
    background-color: rgb(240, 240, 240);
  }
</style>
