<template>
  <div class="node">
    <div class="node-info"  v-if="config.showFooter !== undefined">
      <span>循环组件</span>
      <i 
        class="el-icon-close loop-del-btn"
        title="删除循环组件" 
        @click.stop="$emit('delNode', true)"
      ></i>
    </div>
    <div v-if="config.showFooter === undefined || config.showFooter" class="node-footer">
      <div class="btn"  @dragover="handleDragover" @drop="handleDrop">
        <insert-button @insertNode="type => $emit('insertNode', type)"></insert-button>
      </div>
    </div>
  </div>
</template>

<script>
import InsertButton from '@/views/process/node/InsertButton.vue'

export default {
  name: "LoopNode",
  props:{
    config:{
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  components: {InsertButton},
  data() {
    return {
      showError: false,
      errorInfo: '',
    }
  },
  computed:{
    content(){
      const config = this.config.props
      switch (config.assignedType){
        case "ASSIGN_USER":
          if (config.assignedUser.length > 0){
            let texts = []
            config.assignedUser.forEach(org => texts.push(org.name))
            return String(texts).replaceAll(',', '、')
          }else {
            return '请指定审批人'
          }
        case "SELF":
          return '发起人自己'
        case "SELF_SELECT":
          return config.selfSelect.multiple ? '发起人自选多人':'发起人自选一人'
        case "LEADER_TOP":
          return '多级主管依次审批'
        case "LEADER":
          return config.leader.level > 1 ? '发起人的第 ' + config.leader.level + ' 级主管' : '发起人的直接主管'
        case "FORM_USER":
          if (!config.formUser || config.formUser === ''){
            return '表单内联系人（未选择）'
          }else {
            let text = this.getFormItemById(config.formUser)
            if (text && text.title){
              return `表单（${text.title}）内的人员`
            }else {
              return '该表单已被移除😥'
            }
          }
        case "ROLE":
          if (config.role.length > 0){
            return String(config.role).replaceAll(',', '、')
          }else {
            return '指定角色（未设置）'
          }
          default: return '未知设置项😥'
      }
    }
  },
  methods: {
    handleDragover(event) {
      event.preventDefault()
    },
    handleDrop(event) {
      event.preventDefault()
      console.log("🚀 ~ file: InsertButton.vue:74 ~ handleDrop ~ event:", event)
      this.$emit('insertNode', event.dataTransfer.getData("text/plain"))
    },

    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(err){
      try {
        return this.showError = !this[`validate_${this.config.props.assignedType}`](err)
      } catch (e) {
        return true;
      }
    },
    validate_ASSIGN_USER(err){
      if(this.config.props.assignedUser.length > 0){
        return true;
      }else {
        this.errorInfo = '请指定审批人员'
        err.push(`${this.config.name} 未指定审批人员`)
        return false
      }
    },
    validate_SELF_SELECT(err){
      return true;
    },
    validate_LEADER_TOP(err){
      return true;
    },
    validate_LEADER(err){
      return true;
    },
    validate_ROLE(err){
      if (this.config.props.role.length <= 0){
        this.errorInfo = '请指定负责审批的系统角色'
        err.push(`${this.config.name} 未指定审批角色`)
        return false
      }
      return true;
    },
    validate_SELF(err){
      return true;
    },
    validate_FORM_USER(err){
     if (this.config.props.formUser === ''){
       this.errorInfo = '请指定表单中的人员组件'
       err.push(`${this.config.name} 审批人为表单中人员，但未指定`)
       return false
     }
      return true;
    },
    validate_REFUSE(err){
      return true;
    },
  }
}
</script>

<style lang="less" scoped>
@import "@/assets/theme";

.node{
  padding: 30px 55px 0;
  width: 220px;
  .node-body{
    overflow: hidden;
    cursor: pointer;
    min-height: 80px;
    max-height: 120px;
    position: relative;
    border-radius: 5px;
    background-color: white;
    box-shadow: 0px 0px 5px 0px #d8d8d8;
    &:hover{
      .node-body-main {
        .option{
          display: inline-block !important;
        }
      }
      box-shadow: 0px 0px 3px 0px @primary;
    }
  
    .node-body-main {
      position: absolute;
      width: 188px;
      left: 17px;
      display: inline-block;

      .node-body-main-header{
        padding: 10px 0px 5px;
        font-size: xx-small;
        position: relative;
        .title{
          color: #718dff;
        }
        .option{
          position: absolute;
          right: 0;
          display: none;
          font-size: medium;
          i{
            color: #888888;
            padding: 0 3px;
          }
        }
      }
      .node-body-main-content {
        padding: 6px;
        color: #656363;
        font-size: 14px;

        i {
          position: absolute;
          top: 55%;
          right: 10px;
          font-size: medium;
        }
      }
    }
  }

  .node-footer{
    position: relative;
    .btn{
      width: 100%;
      display: flex;
      height: 70px;
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
  
  .node-info {
    position: absolute;
    top: 0;
    left: 50%;
    padding: 2px 4px;
  }
  .loop-del-btn{
    color: #888888;
    padding: 0 3px;
    cursor: pointer;
    z-index: 2;
    display: none;
  }
}
</style>
