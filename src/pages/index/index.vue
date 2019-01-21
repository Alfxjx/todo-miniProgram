
<template>
  <div class="container">
    <div class="weui-cells__title">todo</div>
    <div class="weui-cells weui-cells_after-title">
      <div class="weui-cell weui-cell_input">
        <div class="weui-cell__bd">
          <input class="weui-input" placeholder="add a todo" v-model="todoItem"/>
        </div>
      </div>
    </div>
    <button class="weui-btn add" type="primary" @click="addTODO()">Add</button>
    <div class="list">待办列表</div>
    <ul v-if="todoList" class="done-list">
      <li class="item-content" v-for="(item , index) in todoList " :key="index">
        <span @click="done(item)" :class="{done:item.isDone}">{{item.label}}</span>
      </li>
    </ul>
    <button class="weui-btn save" type="primary" @click="save">保存</button>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        todoItem: '',
        todoList: []
      }
    },
    methods: {
      addTODO () {
        if (this.todoItem) {
          console.log(this.todoItem)
          this.todoList.push({
            label: this.todoItem,
            isDone: false
          })
          this.todoItem = ''
        }
      },
      done (item) {
        item.isDone = !item.isDone
      }
    }
  }
</script>

<style scoped lang="stylus">
  .container
    .add
      width 400rpx
    .list
      color #0bb20c
      text-align center
    .done-list
      margin 60rpx 15rpx 15rpx
      .item-content
        padding 10rpx
        margin-bottom 10rpx
        text-align center
        border 1px solid black
        border-radius 35rpx
        .done
          text-decoration line-through
    .save
      position absolute
      bottom 0rpx
      width 100%
</style>
