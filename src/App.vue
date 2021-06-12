<template>
  <!--<HelloWorld msg="Hello Vue 3.0 + Vite" />-->
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo" />
      <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
      <Footer :todos="todos"
        :checkAll="checkAll"
        :clearAllCompletedTodos="clearAllCompletedTodos" />
    </div>
  </div>
</template>

<script>
import { defineComponent, onMounted, reactive, toRefs, watch } from 'vue'
// import HelloWorld from './components/HelloWorld.vue'
import Footer from './components/Footer.vue'
import Header from './components/Header.vue'
import List from './components/List.vue'

export default defineComponent({
  name: 'App',
  components: {
    // HelloWorld,
    Footer,
    Header,
    List
  },
  setup() {
    // 从浏览器的缓存中设置数据
    const saveTodos = (todos)=> {
      localStorage.setItem('todos_key', JSON.stringify(todos))
    }
    // 从浏览器的缓存中读取数据
    const readTodos = ()=> {
      return JSON.parse(localStorage.getItem('todos_key') || '[]')
    }

    const state = reactive({
      todos: [],
    })

    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos()
      }, 1000)
    })

    const addTodo = (todo)=>{
      let flag = false;
      state.todos.forEach((item)=> {
        if(item.title === todo.title) {
          flag = true;
          alert("重复了");
        }
      })

      if(!flag) {
        state.todos.unshift(todo)
      }
    };

    const deleteTodo = (index)=>{
      state.todos.splice(index, 1)
    };

    const updateTodo = (todo, isCompleted)=>{
      todo.isCompleted = isCompleted
    };

     // 全选或者是全不选的方法
    const checkAll = (isCompleted) => {
      state.todos.forEach((todo) => {
        todo.isCompleted = isCompleted
      })
    }
    // 清理所有选中的数据
    const clearAllCompletedTodos = () => {
      state.todos = state.todos.filter((todo) => !todo.isCompleted)
    }

    watch(() => state.todos, saveTodos, { deep: true })

    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAll,
      clearAllCompletedTodos
    }
  }
})
</script>
<style scoped>
/*app*/
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>