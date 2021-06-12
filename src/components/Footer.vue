<template>
  <div class="todo-footer">
    <div >
      <label>
        <input type="checkbox" v-model="isCheckAll" />
      </label>
      <span>
        <span>已完成{{ count }}</span> / 全部{{ todos.length }}
      </span>
    </div>
    <button class="btn btn-danger" @click="clearAllCompletedTodos">
      清除已完成任务
    </button>
  </div>
</template>
<script>
import { defineComponent, computed } from 'vue'
export default defineComponent({
  name: 'Footer',
  props: {
    checkAll: {
      type: Function,
      required: true,
    },
    todos: {
      type: Array,
      required: true,
    },
    clearAllCompletedTodos: {
      type: Function,
      required: true,
    }
  },
  setup(props) {

    const count = computed(()=>{
      return props.todos.reduce(
        (pre, todo, index) => pre + (todo.isCompleted ? 1 : 0),
        0
      )
    })

    const isCheckAll = computed({
      get() {
        return count.value > 0 && props.todos.length === count.value
      },
      set(val) {
        props.checkAll(val)
      },
    })

    return {
      count,
      isCheckAll
    }
  }
})
</script>
<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  margin-top: 5px;
}

.btn-danger {
  padding: 10px 15px;
  border-radius: 10px;
  background-color: red;
  color: white;
  border: none;
  outline: none;
  cursor: pointer;
}
</style>