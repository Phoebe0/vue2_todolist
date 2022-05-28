<template>
  <section class="todoapp">
    <TodoHeader @add="addFn"></TodoHeader>
    <!-- 1. 父组件传递数据给子组件 -->
    <TodoMain
      :list="showList"
      @del="delFn"
      @changeDone="changeFn"
      @changeAll="changeAllFn"
    ></TodoMain>
    <TodoFooter
      :list="list"
      @clear="clearFn"
      :menus="menus"
      :active="active"
      @changeActive="changeActive"
    ></TodoFooter>
  </section>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoMain from './components/TodoMain'
import TodoFooter from './components/TodoFooter'
export default {
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter
  },
  data() {
    return {
      // 提供任务数据
      // 如果没有数据，会返回null，所以要加一个空数组
      list: JSON.parse(localStorage.getItem('list')) || [],
      // 提供切换的菜单
      menus: ['所有', '未完成', '已完成'],
      // 默认高亮的菜单
      active: '所有'
    }
  },
  methods: {
    delFn(id) {
      // console.log('父组件接受到', id)
      this.list = this.list.filter((item) => item.id !== id)
    },
    changeFn(id) {
      // console.log('父组件接收', id)
      const obj = this.list.find((item) => item.id === id)
      obj.isDone = !obj.isDone
    },
    addFn(name) {
      console.log('父组件接收', name)
      this.list.unshift({
        id: Date.now(),
        name,
        isDone: false
      })
    },
    changeAllFn(value) {
      // 让list中所有的任务的isDone属性修改为value
      this.list.forEach((item) => (item.isDone = value))
    },
    clearFn() {
      // 保留未完成的任务
      this.list = this.list.filter((item) => item.isDone === false)
    },
    changeActive(item) {
      this.active = item
    }
  },
  computed: {
    // 用于显示的list
    showList() {
      if (this.active === '未完成') {
        return this.list.filter((item) => item.isDone === false)
      } else if (this.active === '已完成') {
        return this.list.filter((item) => item.isDone === true)
      } else {
        // 所有的
        return this.list
      }
    }
  },
  watch: {
    // 复杂类型，需要使用完整写法
    list: {
      handler(value) {
        // 使用本地存储
        localStorage.setItem('list', JSON.stringify(value))
      },
      // 深度监听
      deep: true
    }
  }
}
</script>

<style></style>
