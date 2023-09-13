<template>
  <node :title="config.name" :show-error="showError" :content="content" :error-info="errorInfo"
        @selected="$emit('selected')" @delNode="$emit('delNode')" @insertNode="type => $emit('insertNode', type)"
        placeholder="请设置变量" header-bgc="#f314e0" header-icon="el-icon-position"/>
</template>

<script>
import Node from './Node'

export default {
  name: "VariableNode",
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
      const list = this.config.props.variableList || []
      const arr = list.filter(item => item.name).map(item => {
        return `${item.name}=${item.value || undefined}`
      })
      return arr.join(';')
    }
  },
  methods: {
    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(){
      const hasEmpty = this.config.props.variableList.length === 0  || this.config.props.variableList.find(item => !item.name)
      this.showError = !!hasEmpty
      if(hasEmpty) {
        this.errorInfo = '请设置变量'
      }
      return !this.showError
    }
  }
}
</script>

<style scoped>

</style>
