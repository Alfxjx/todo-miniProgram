
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
    <button class="weui-btn add" type="primary" @click="addTODO()">添加</button>
    <button class="weui-btn save" type="primary" @click="save">保存</button>
    <button class="weui-btn del" type="warn" disabled="true" @click="del">删除</button>
    <div class="list-title">待办列表</div>
    <div class="list-wrapper">
      <ul class="done-list">
        <li class="item-content" v-for="(item , index) in todoList " :key="index" @click="done(item)">
          <span :class="{done:item.isDone}">{{item.label}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

  const user = 'localUser'
  export default {
    data () {
      return {
        todoItem: '',
        todoList: []
      }
    },
    created(){
      let that = this
      wx.getStorage({
        key:user,
        success(res){
          console.log(res.data[0])
          for(let i=0;i<res.data.length;i++){
            that.todoList.unshift(res.data[i])
          }
        }
      })
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
      },
      save(){
        console.log('saved')
        wx.setStorage({key:user,data:this.todoList})
      },
      del(){
        console.log('delete')
        wx.removeStorage({
          key: user,
          success(res){
            console.log(res.data)
          }
        })
      }
    }
  }
</script>

<style scoped lang="stylus">
  .container
    position relative
    .add
      display inline-block
      margin-left 10%
      width 200rpx
    .save
      display inline-block
      margin-left 20rpx
      width 200rpx
    .del
      display inline-block
      margin-left 20rpx
      width 200rpx
    .list-title
      color #0bb20c
      text-align center
    .list-wrapper
      position absolute
      top 290rpx
      left 0
      width 100%
      overflow hidden
      .done-list
        margin 60rpx 15rpx 15rpx
        .item-content
          padding 10rpx
          margin-bottom 10rpx
          text-align center
          border 1px solid grey
          border-radius 35rpx
          .done
            text-decoration line-through
</style>
