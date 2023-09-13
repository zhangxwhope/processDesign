<template>
  <node :title="config.name" :show-error="showError" :content="content" :error-info="errorInfo"
        @selected="$emit('selected')" @delNode="$emit('delNode')" @insertNode="type => $emit('insertNode', type)"
        placeholder="请设置xml" header-bgc="#f25643" header-icon="el-icon-folder-delete"/>
</template>

<script>
import Node from './Node'

export default {
  name: "XmlNode",
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
      return '请设置xml'
    }
  },
  methods: {
    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(){
      const hasEmpty = !this.config.props.xml
      this.showError = !!hasEmpty
      if(hasEmpty) {
        this.errorInfo = '请设置xml'
      }
      return !this.showError
    }
  }
}
</script>

<style scoped>

</style>
