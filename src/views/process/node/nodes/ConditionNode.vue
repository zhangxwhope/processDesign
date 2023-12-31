<template>
  <div :class="{'node': true, 'node-error-state': showError}">
    <div :class="{'node-body': true, 'error': showError}">
      <div class="body-wrapper">
        <div class="node-body-left" @click="$emit('leftMove')" v-if="level > 1">
          <i class="el-icon-arrow-left"></i>
        </div>
        <div class="node-body-main" @click="$emit('selected')">
          <div class="node-body-main-header">
            <span class="title">{{ config.name ? config.name : ('条件' + level) }}</span>
            <span class="level">优先级{{ level }}</span>
            <span class="option">
              <el-tooltip effect="dark" content="复制条件" placement="top">
                <i class="el-icon-copy-document" @click.stop="$emit('copy')"></i>
              </el-tooltip>
              <i class="el-icon-close" @click.stop="$emit('delNode')"></i>
            </span>
          </div>
          <div class="node-body-main-content">
            <span class="placeholder" v-if="(content || '').trim() === ''">{{ placeholder }}</span>
            <ellipsis hoverTip :row="4" :content="content" v-else/>
          </div>
        </div>
        <div class="node-body-right" @click="$emit('rightMove')" v-if="level < size">
          <i class="el-icon-arrow-right"></i>
        </div>
        <div class="node-error" v-if="showError">
          <el-tooltip effect="dark" :content="errorInfo" placement="top-start">
            <i class="el-icon-warning-outline"></i>
          </el-tooltip>
        </div>
      </div>
    </div>
    <div class="node-footer">
      <div class="btn"  @dragover="handleDragover" @drop="handleDrop">
        <insert-button @insertNode="type => $emit('insertNode', type)"></insert-button>
      </div>
    </div>
  </div>
</template>

<script>
import InsertButton from '@/views/process/node/InsertButton.vue'
import ValueType from '@/views/process/ConditionVarType'

export default {
  name: "ConditionNode",
  components: {InsertButton},
  props: {
    config: {
      type: Object,
      default: () => {
        return {}
      }
    },
    //索引位置
    level: {
      type: Number,
      default: 1
    },
    //条件数
    size: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      ValueType,
      placeholder: '请设置条件',
      errorInfo: '',
      showError: false
    }
  },
  computed: {
    content() {
      const groups = this.config.props.groups
      let confitions = []
      groups.forEach(group => {
        let subConditions = []
        group.conditions.forEach(subCondition => {
          let subConditionStr = ''
          switch (subCondition.valueType) {
            case ValueType.dept:
            case ValueType.user:
              subConditionStr = `${subCondition.title}属于[${String(subCondition.value.map(u => u.name)).replaceAll(',', '. ')}]之一`
              break;
            case ValueType.number:
              subConditionStr = this.getNumberConditionContent(subCondition)
              break;
            case ValueType.string:
              subConditionStr = `${subCondition.title}为${subCondition.value[0]}`
              break;
          }
          subConditions.push(subConditionStr)
        })
        //根据子条件关系构建描述
        let subConditionsStr = String(subConditions)
            .replaceAll(',', subConditions.length > 1 ?
                (group.groupType === 'AND' ? ') 且 (' : ') 或 (') :
                (group.groupType === 'AND' ? ' 且 ' : ' 或 '))
        confitions.push(subConditions.length > 1 ? `(${subConditionsStr})` : subConditionsStr)
      })
      //构建最终描述
      return String(confitions).replaceAll(',', (this.config.props.groupsType === 'AND' ? ' 且 ' : ' 或 '))
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
    
    getNumberConditionContent(subCondition) {
      switch (subCondition.compare) {
        case 'IN':
          return `${subCondition.title}为[${String(subCondition.value)}]中之一`
        case 'B':
          return `${subCondition.value[0]} < ${subCondition.title} < ${subCondition.value[1]}`
        case 'AB':
          return `${subCondition.value[0]} ≤ ${subCondition.title} < ${subCondition.value[1]}`
        case 'BA':
          return `${subCondition.value[0]} < ${subCondition.title} ≤ ${subCondition.value[1]}`
        case 'ABA':
          return `${subCondition.value[0]} ≤ ${subCondition.title} ≤ ${subCondition.value[1]}`
        case '<=':
          return `${subCondition.title} ≤ ${subCondition.value[0]}`
        case '>=':
          return `${subCondition.title} ≥ ${subCondition.value[0]}`
        default:
          return `${subCondition.title}${subCondition.compare}${subCondition.value[0]}`
      }
    },
    //校验数据配置的合法性
    validate() {
      if (this.config.assignedUser && this.config.assignedUser.length > 0) {
        this.showError = false
      } else {
        this.showError = true
        this.errorInfo = '请选择需要抄送的人员'
      }
      return !this.showError
    }
  }
}
</script>

<style lang="less" scoped>
@import "@/assets/theme";

.node-error-state {
  .node-body {
    box-shadow: 0px 0px 5px 0px #F56C6C !important;
  }
}

.node {
  padding: 30px 55px 0;
  width: 220px;

  .node-body {
    cursor: pointer;
    min-height: 80px;
    max-height: 120px;
    position: relative;
    border-radius: 5px;
    background-color: white;
    box-shadow: 0px 0px 5px 0px #d8d8d8;

    &:hover {
      .node-body-left, .node-body-right {
        i {
          display: block !important;
        }
      }

      .node-body-main {
        .level {
          display: none !important;
        }

        .option {
          display: inline-block !important;
        }
      }

      box-shadow: 0px 0px 3px 0px @primary;
    }

    .node-body-left, .node-body-right {
      display: flex;
      align-items: center;
      position: absolute;
      height: 100%;

      i {
        display: none;
      }

      &:hover {
        background-color: #ececec;
      }
    }

    .node-body-left {
      left: 0;
    }

    .node-body-right {
      right: 0;
      top: 0;
    }

    .node-body-main {
      //position: absolute;
      width: 188px;
      margin-left: 17px;
      display: inline-block;

      .node-body-main-header {
        padding: 10px 0px 5px;
        font-size: xx-small;
        position: relative;

        .title {
          color: #15bca3;
        }

        .level {
          position: absolute;
          right: 15px;
          color: #888888;
        }

        .option {
          position: absolute;
          right: 0;
          display: none;
          font-size: medium;

          i {
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

        .placeholder {
          color: #8c8c8c;
        }
      }
    }

    .node-error {
      position: absolute;
      right: -40px;
      top: 20px;
      font-size: 25px;
      color: #F56C6C;
    }
  }

  .body-wrapper{
    min-height: 80px;
    background: #fff;
  }

  .node-footer {
    position: relative;

    .btn {
      width: 100%;
      display: flex;
      height: 70px;
      padding: 20px 0 32px;
      justify-content: center;
    }

    /deep/ .el-button {
      height: 32px;
    }

    &::before {
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
