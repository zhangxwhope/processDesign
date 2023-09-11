<template>
  <div class="process-start">
    <div class="node-content">{{ content }}</div>
    <div class="node-footer">
      <div class="btn">
        <insert-button @insertNode="type => $emit('insertNode', type)"></insert-button>
      </div>
    </div>
  </div>
</template>

<script>
import InsertButton from '@/views/process/node/InsertButton.vue'

export default {
  name: "RootNode",
  components: {InsertButton},
  props:{
    config:{
      type: Object,
      default: () => {
        return {}
      }
    },
    content: {
      type: String,
      default: '开始'
    }
  },
  data() {
    return {
    }
  },
  methods: {}
}
</script>

<style scoped lang="less">
.process-start{
  .node-content{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 2px solid red;
  }
  .node-footer{
    position: relative;
    .btn{
      width: 100%;
      display: flex;
      padding: 20px 0 32px;
      justify-content: center;
    }
    /deep/ .el-button{
      height: 32px;
    }
    &::before{
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: -1;
      margin: auto;
      width: 2px;
      height: 100%;
      background-color: #CACACA;
    }
  }
}
</style>
