<template>
  <div>
    <el-tabs v-model="active" v-if="name && formConfig.length > 0">
      <el-tab-pane :label="name" name="properties">
        <component :is="(selectNode.type||'').toLowerCase()" :config="selectNode.props"/>
      </el-tab-pane>
      <el-tab-pane label="表单权限设置" name="permissions">
        <form-authority-config/>
      </el-tab-pane>
    </el-tabs>
    <component :is="(selectNode.type||'').toLowerCase()" v-else :config="selectNode.props"/>
  </div>
</template>

<script>
import Variable from './VariableNodeConfig.vue'
import Api from './APINodeConfig.vue'
import Timer from './TimerNodeConfig.vue'
import Dynamic from './DynamicNodeConfig.vue'
import String from './StringNodeConfig.vue'
import Xml from './XmlNodeConfig.vue'
import Json from './JsonNodeConfig.vue'
import Concurrent from './ConcurrentNodeConfig.vue'

import Approval from './ApprovalNodeConfig.vue'
import Condition from './ConditionNodeConfig.vue'
import Delay from './DelayNodeConfig.vue'
import Cc from './CcNodeConfig.vue'
import Trigger from './TriggerNodeConfig.vue'
import FormAuthorityConfig from './FormAuthorityConfig.vue'
import Root from './RootNodeConfig.vue'

export default {
  name: "NodeConfig",
  components: {
    Variable,
    Api,
    Timer,
    Dynamic,
    String,
    Xml,
    Json,
    Concurrent,
    
    Approval,
    Condition,
    Trigger,
    Delay,
    Root,
    Cc,
    FormAuthorityConfig
  },
  data() {
    return {
      active: 'properties',
    }
  },
  computed: {
    selectNode() {
      return this.$store.state.selectedNode
    },
    formConfig() {
      return this.$store.state.design.formItems
    },
    name() {
      switch (this.selectNode.type) {
        case 'ROOT':
          return '设置发起人';
        case 'APPROVAL':
          return '设置审批人';
        default:
          return null;
      }
    }
  },
  methods: {}
}
</script>

<style lang="less" scoped>

</style>
