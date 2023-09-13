<template>
  <node :title="config.name" :show-error="showError" :content="content" :error-info="errorInfo"
        @selected="$emit('selected')" @delNode="$emit('delNode')" @insertNode="type => $emit('insertNode', type)"
        placeholder="请设置动态脚本" header-bgc="rgb(21, 188, 131)" header-icon="el-icon-c-scale-to-original"/>
</template>

<script>
import Node from './Node'

export default {
  name: "DynamicNode",
  props:{
    config:{
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  components: {Node},
  data() {
    return {
      showError: false,
      errorInfo: '',
    }
  },
  computed:{
    content(){
      // const config = this.config.props
      return '请设置动态脚本'
    }
  },
  methods: {
    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(){
      const hasEmpty = !this.config.props.script
      this.showError = !!hasEmpty
      if(hasEmpty) {
        this.errorInfo = '请设置动态脚本'
      }
      return !this.showError
    }
  }
}
</script>

<style scoped>

</style>
