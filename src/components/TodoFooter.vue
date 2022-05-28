<template>
  <footer class="footer" v-if="list.length > 0">
    <!-- This should be `0 items left` by default -->
    <span class="todo-count">
      <strong>{{ leftCount }}</strong> item left
    </span>
    <!-- Remove this if you don't implement routing -->
    <ul class="filters">
      <li v-for="item in menus" :key="item">
        <!-- class="selected" -->
        <a
          :class="{ selected: item === active }"
          href="#/"
          @click.prevent="fn(item)"
          >{{ item }}</a
        >
      </li>
    </ul>
    <!-- Hidden if no completed items are left ↓ -->
    <button class="clear-completed" @click="clear">Clear completed</button>
  </footer>
</template>

<script>
export default {
  // 接收数据
  props: {
    list: {
      type: Array,
      // 如果提供的默认值是简单类型：  defautl: 100
      // 如果提供的默认值是复杂类型，必须是一个函数的格式，函数返回数据
      // default: () => []
      required: true
    },
    menus: {
      type: Array,
      required: true
    },
    active: {
      type: String,
      required: true
    }
  },
  computed: {
    // left 剩余  count: 数量
    // leftCount: 统计list中所有没有完成的任务的数量接口
    leftCount() {
      return this.list.filter((item) => item.isDone === false).length
    }
  },
  methods: {
    clear() {
      // 把list中所有完成的任务清除，保留未完成的任务
      // console.log('清空')
      this.$emit('clear')
    },
    fn(item) {
      // console.log('我点了', item)
      // 传递给父组件
      this.$emit('changeActive', item)
    }
  }
}
</script>

<style></style>
