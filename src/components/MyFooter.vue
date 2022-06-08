<template>
  <div class="footer" v-show="todos.length">
    <span>
      <input type="checkbox" :checked="dones === todos.length" @change="doneAll"/> 
      <span>完成 {{dones}} / 总共 {{todos.length}}</span>
    </span> 
    <button @click="clearAllDone">清空已完成</button>
  </div>
</template>

<script>
  export default {
    name: 'MyFooter',
    props: ['todos'],
    computed: {
      dones(){
        return this.todos.reduce((pre, item)=>{
          if(item.done) {
            pre++
          }
          return pre
        }, 0)
      }
    },
    methods: {
      doneAll(e){
        this.$bus.$emit('checkAllTodo', e.target.checked)
      },
      clearAllDone(){
        this.$bus.$emit('clearAllDone')
      }
    }
  }
</script>

<style scoped>
  .footer {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    width: 100%;
    height: 30px;
  }
</style>