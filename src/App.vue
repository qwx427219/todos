<template>
  <div class="container">
    <MyHeader></MyHeader>
    <MyContent :todos="todos"></MyContent>
    <MyFooter :todos="todos"></MyFooter>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader.vue'
  import MyContent from './components/MyContent.vue'
  import MyFooter from './components/MyFooter.vue'

  import {v1} from 'uuid'
  import moment from 'moment'

  export default {
    name: 'App',
    components: {
      MyHeader,
      MyContent,
      MyFooter
    },
    data(){
      return {
        todos: []
      }
    },
    methods: {
      addTodo(name){
        const todo = {id:v1(), name, createTime: moment().format('YYYY-MM-DD HH:mm:ss'), done:false}
        this.todos.unshift(todo)
      },
      delTodo(id){
        this.todos = this.todos.filter(todo => {
          return todo.id !== id
        })
      },
      changeTodo(id, todo){
        const index = this.todos.findIndex(todo => {
          return todo.id === id
        })
        if(Object.prototype.hasOwnProperty.call(todo, 'done')) this.todos[index].done = todo.done
        if(Object.prototype.hasOwnProperty.call(todo, 'name')) this.todos[index].name = todo.name
      },
      checkAllTodo(doneAll){
        this.todos.forEach(todo => {
          todo.done = doneAll
        })
      },
      clearAllDone(){
        this.todos = this.todos.filter(todo => {
          return !todo.done
        })
      }
    },
    watch: {
      todos: {
        deep: true,
        handler(val){
          localStorage.setItem('todos', JSON.stringify(val))
        }
      }
    },
    mounted(){
      const todos = localStorage.getItem('todos')
      if(todos) this.todos = JSON.parse(todos)

      this.$bus.$on('addTodo', this.addTodo)
      this.$bus.$on('delTodo', this.delTodo)
      this.$bus.$on('changeTodo', this.changeTodo)
      this.$bus.$on('checkAllTodo', this.checkAllTodo)
      this.$bus.$on('clearAllDone', this.clearAllDone)
    },
    beforeDestroy(){
      this.$bus.$off('addTodo')
      this.$bus.$off('delTodo')
      this.$bus.$off('changeTodo')
      this.$bus.$off('checkAllTodo')
      this.$bus.$off('clearAllDone')
    }
  }
</script>

<style>
  body {
    display: flex;
    justify-content: space-evenly;
  }

  .container {
    width: 500px;
    background-color: #ccc;
    padding: 5px;
    border-radius: 10px;
  }
</style>
