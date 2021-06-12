<template>
  <li
    @mouseenter="mouseHandler(true)"
    @mouseleave="mouseHandler(false)"
    :style="{ backgroundColor: bgColor, color: myColor }"
  >
    <label>
      <input type="checkbox" v-model="isComptete" />
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click="delTodo">
      删除
    </button>
  </li>
</template>
<script>
import { defineComponent, ref, computed } from 'vue'
export default defineComponent({
  name: 'Item',
  props: {
    todo: {
      type: Array,
      required: true,
    },
    deleteTodo: {
      type: Function,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    updateTodo: {
      type: Function,
      required: true,
    },
  },
  setup(props) {

    const bgColor = ref('white')
    const myColor = ref('black')
    const isShow = ref(false)

    const mouseHandler = (flag)=> {
      if(flag) {
        bgColor.value = 'pink'
        myColor.value = 'white'
        isShow.value = true
      } else {
        bgColor.value = 'white'
        myColor.value = 'black'
        isShow.value = false
      }
    }

    const delTodo = () => {
      // 提示
      if (window.confirm('确定要删除吗?')) {
        props.deleteTodo(props.index)
      }
    }

    const isComptete = computed({
      get() {
        return props.todo.isCompleted
      },
      set(val) {
        props.updateTodo(props.todo, val)
      },
    })

    return {
      bgColor,
      myColor,
      isShow,
      isComptete,
      mouseHandler,
      delTodo
    }
  }
})
</script>
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  /* display: none; */
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>