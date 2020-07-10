<template>
  <div>
      <input type="text" v-model="newTodo" class="todo-input" placeholder="What will we do next?..." @keyup.enter="addTodo">
      <div class="todo-item" v-for="(todo, index) in filterTodo" :key="todo.id">
              <div  class="todo-item-left">
                    <input type="checkbox" v-model="todo.completed">
                    <div v-if="!todo.editing" class="todo-label" :class="{ completed : todo.completed }" @dblclick="editTodo(todo)">
                        {{todo.title}}
                    </div>
                    <input v-else v-focus class="todo-edit-input" type="text" v-model="todo.title" @keyup.esc="cancelEdit(todo)" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)">         
              </div>
              <div class="remove-todo" @click="removeTodo(index)">
                  &times;
              </div>          
      </div>
      <div class="extra-container">
          <div>
              <label><input type="checkbox" :checked="checkRemaining" @change="doneAllTodo">Check All</label>
          </div>
          <div>
              {{ remainingTodo }} todos left
          </div>
      </div>
      <div class="extra-container">    
          <div>
              <button :class="{buttonActive: todoFilter == 'all' }" @click="todoFilter = 'all'">All</button>
              <button :class="{buttonActive: todoFilter == 'undone'}" @click="todoFilter = 'undone'">Undone</button>
              <button :class="{buttonActive: todoFilter == 'completed'}" @click="todoFilter = 'completed'">Completed</button>
          </div>
          <div>
              <button v-if="showRemoveCompletedButton" @click="removeCompletedTodo">Remove Completed</button>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    data () {
        return {
            newTodo: '',
            idForTodo: 4,
            beforeEditCache:  "",
            todoFilter: "all",
            todos : [
                {
                    id: 1,
                    title: 'Taking some laundry',
                    completed: false,
                    editing: false
                },
                {
                    id: 2,
                    title: 'Buy cat foods',
                    completed: false,
                    editing: false
                },
                {
                    id: 3,
                    title: 'Make a todo app using Vue.js',
                    completed: false,
                    editing: false
                }
            ]
        }
    },
    directives: {
        focus: {
            inserted: function(el) {
                el.focus();
            }
        }
    },
    methods: {
        addTodo () {
            if(this.newTodo.trim() == ''){
                return
            }
            this.todos.push({
                id: this.idForTodo,
                title: this.newTodo,
                completed: false,
                editing: false
            });
            this.newTodo = '';
            this.idForTodo++;
        },
        removeTodo (index) {
            this.todos.splice(index, 1);
        },
        editTodo (todo) {
            this.beforeEditCache = todo.title;
            todo.editing = true;
        },
        cancelEdit (todo) {
            todo.editing = false;
            todo.title = this.beforeEditCache;
        },
        doneEdit (todo) {
            todo.editing = false;
        },
        doneAllTodo () {
            this.todos.forEach(todo => todo.completed = !todo.completed);
        },
        removeCompletedTodo () {
            this.todos = this.todos.filter(todo => !todo.completed);
        }
    },
    computed: {
        remainingTodo () {
            return this.todos.filter(todo => !todo.completed).length
        },
        filterTodo () {
            if (this.todoFilter == 'all') {
                return this.todos;
            } else if (this.todoFilter == 'undone') {
                return this.todos.filter(todo => !todo.completed);
            } else if (this.todoFilter == 'completed') {
                return this.todos.filter(todo => todo.completed);
            }
            return this.todos;
        },
        checkRemaining () {
            // return this.remainingTodo == 0;
            if (this.remainingTodo == 0){
                return true;
            }else {
                return false
            }
        },
        showRemoveCompletedButton () {
            return this.todos.filter(todo => todo.completed).length > 0
        }
    }
}
</script>

<style scoped>
    .todo-input {
        width: 100%;
        padding: 8px 8px;
        border: 1px solid #ccc;
        outline-color: #ccc;
        color: #35495E;
        font-size: 20px;
        margin-bottom: 23px;
    }

    .todo-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 24px;
        font-weight: 500;
        margin-bottom: 6px;
       
    }

    .remove-todo:hover {
        color: red;
        cursor: pointer;
    }

    .completed {
        text-decoration: line-through;
    }

    .todo-edit-input {
        width: 100%;
        padding: 8px 8px;
        border: 1px solid #ccc;
        outline-color: #ccc;
        color: #35495E;
        font-size: 24px;
        margin-bottom: 23px;
    }

    .todo-label {
         margin-left: 5px;
    }
    
    .todo-item-left {
        display: flex;
        align-items: center;
        
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-top: 24px;
        border-top: 1px solid grey;
        padding-top: 12px;
    }

    button {
        padding: 7px 7px;
        margin-left: 3px;
    }

    button:hover {
        background-color: #41B883;
        color: white;
        border: 1px solid black;
    }

    .buttonActive {
        background-color: #41B883;
        color: white;
        border: 1px solid black;
    }

</style>
