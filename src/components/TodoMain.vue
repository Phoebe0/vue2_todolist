<template>
  <section class="main">
    <input
      id="toggle-all"
      class="toggle-all"
      type="checkbox"
      v-model="checkAll"
    />
    <label for="toggle-all">Mark all as complete</label>
    <ul class="todo-list">
      <li
        :class="{ completed: item.isDone }"
        v-for="item in list"
        :key="item.id"
      >
        <div class="view">
          <!-- 不希望直接修改父组件的数据，不使用v-model="item.isDone" -->
          <input
            class="toggle"
            type="checkbox"
            :checked="item.isDone"
            @change="changeDone(item.id)"
          />
          <label>{{ item.name }}</label>
          <!-- 删除按钮 -->
          <button class="destroy" @click="del(item.id)"></button>
        </div>
        <input class="edit" value="Create a TodoMVC template" />
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      required: true
    }
  },
  computed: {
    checkAll: {
      get() {
        // 全选的逻辑，当所有的任务都完成，就应该选中
        return this.list.every((item) => item.isDone === true)
      },
      set(value) {
        console.log('修改', value)
        // 能不能直接遍历list修改状态
        this.$emit('changeAll', value)
      }
    }
  },
  methods: {
    del(id) {
      // console.log('需要删除', id)
      // 把id传递给父组件
      this.$emit('del', id)
    },
    changeDone(id) {
      // console.log('修改状态', id)
      this.$emit('changeDone', id)
    }
  }
}
</script>

<style></style>
