<template>
  <node :title="config.name" :show-error="showError" :content="content" :error-info="errorInfo"
        @selected="$emit('selected')" @delNode="$emit('delNode')" @insertNode="type => $emit('insertNode', type)"
        placeholder="请设置请求节点" header-bgc="#ff943e" header-icon="el-icon-message"/>
</template>

<script>
import Node from './Node'

export default {
  name: "APINode",
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
      return '请设置请求节点'
    }
  },
  methods: {
    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(){
      const hasEmpty = !this.config.props.apiPath
      this.showError = !!hasEmpty
      if(hasEmpty) {
        this.errorInfo = '请设置请求节点'
      }
      return !this.showError
    }
  }
}
</script>

<style scoped>

</style>
