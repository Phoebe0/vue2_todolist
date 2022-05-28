<template>
  <header class="header">
    <h1>todos</h1>
    <input
      class="new-todo"
      placeholder="What needs to be done?"
      autofocus
      v-model.trim="name"
      @keyup.enter="add"
    />
  </header>
</template>

<script>
export default {
  data() {
    return {
      name: ''
    }
  },
  methods: {
    add() {
      console.log('回车触发')
      if (this.name === '') return alert('请输入任务名字')
      // console.log(this.name)
      this.$emit('add', this.name)
      // 清空输入框的内容
      this.name = ''
    }
  }
}
</script>

<style></style>
