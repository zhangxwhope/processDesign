<template>
  <div class="process-end">
    <div class="node-content">{{ content }}</div>
  </div>
</template>

<script>

export default {
  name: "EndNode",
  props:{
    config:{
      type: Object,
      default: () => {
        return {}
      }
    },
    content: {
      type: String,
      default: '结束'
    }
  },
  data() {
    return {
    }
  }
}
</script>

<style scoped lang="less">
.process-end{
  .node-content{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 2px solid red;
  }
}
</style>