<template>
  <div>
      <input type="text" class="todo-input" placeholder="Tasks to be done" v-model="newTodo"  @keyup.enter="addTodo">
      <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining">
      </todo-item>

    <div class="extra-container">
        <todo-check-all></todo-check-all>
        <todo-items-remaining></todo-items-remaining>
    </div>

    <div class="extra-container">
        <todo-filtered></todo-filtered>

        <div>
            <transition name="fade">
            <todo-clear-completed :showClearCompletedButton="showClearCompletedButton"></todo-clear-completed>
            </transition>
        </div>

    </div>
  </div>
</template>

<script>
import TodoCheckAll from './TodoCheckAll.vue'
import TodoClearCompleted from './TodoClearCompleted.vue'
import TodoFiltered from './TodoFiltered.vue'
import TodoItem from './TodoItem.vue'
import TodoItemsRemaining from './TodoItemsRemaining.vue'

export default {
  name: 'todo-list',
  components: {
      TodoItem,
      TodoItemsRemaining,
      TodoCheckAll,
      TodoFiltered,
      TodoClearCompleted,
  },
  data() {
      return {
    newTodo: '',
    idForTodo: 3,
    beforeEditCache: '',
    }
  },

  computed: {
      anyRemaining() {
          return this.$store.getters.anyRemaining
      },
      todosFiltered() {
          return this.$store.getters.todosFiltered
      }
  },
  
  methods: {
      addTodo() { 
          if(this.newTodo.trim().length == 0) {
              return
          }

          this.$store.dispatch('addTodo', {
              id:this.idForTodo,
              title: this.newTodo,
      })

      this.newTodo = ''
      this.idForTodo++
    },
   }
}
</script>

<style>
 .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}
    .todo-input:focus {
    outline: 0;
}

 .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    color: rgb(59, 170, 204);
}

 .remove-item {
    cursor: pointer;
    margin-left: 14px;
    color: #000;
}
 .remove-item:hover {
    color: red;
}

 .todo-item-left{
    font-size: 20px;
    display: flex;
    align-items: center;
 }

 .todo-item-edit {
     font-size: 18px;
     color: rgb(31, 175, 31);
     width: 100%;
     padding:8px;
     border: 1px solid #000;
 }
 .todo-item-edit:focus {
     outline: none;
 }  

 .completed {
     text-decoration: line-through;
     color: rgb(211, 109, 109);
 }

 .extra-container {
     display: flex;
     align-items: center;
     justify-content: space-between;
     font-size: 16px;
     border-top: 1px solid lightgray;
     padding-top: 14px;
     margin-bottom: 14px;
 }

 button {
     padding: 5px;
     font-size: 16px;
     background-color: white;
     appearance: none;
     margin-left: 8px;
     font-size: 14px;
     background: rgb(250, 215, 140);
 }
 button:hover {
    background: rgb(255, 230, 176);
 }
 button:focus {
     outline:none;
 }
 .active {
     background: rgb(255, 188, 188);
 } 

 .fade-enter-active, .fade-leave-active {
     transition: opacity .2s;
 }

 .fade-enter, .fade-leave-to {
     opacity: 0;
 }
</style>