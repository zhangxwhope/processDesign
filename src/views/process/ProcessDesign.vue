<template>
  <el-main>
    <div class="scale">
      <el-button icon="el-icon-plus" size="small" @click="scale += 10" :disabled="scale >= 150" circle></el-button>
      <span>{{ scale }}%</span>
      <el-button icon="el-icon-minus" size="small" @click="scale -= 10" :disabled="scale <= 40" circle></el-button>
      <el-button @click="validate">校验流程</el-button>
      <el-button type="primary" @click="save">保存json</el-button>
      <el-button type="primary" @click="saveImg">导出图片</el-button>
    </div>
    <div class="design" :style="'transform: scale('+ scale / 100 +');'">
      <process-side ref="process-side" />
      <process-tree ref="process-tree" @selectedNode="nodeSelected"/>
    </div>
    <el-drawer :title="selectedNode.name" :visible.sync="showConfig"
               :size="selectedNode.type === 'CONDITION' ? '600px':'500px'"
               direction="rtl" :modal="false" destroy-on-close>
      <div slot="title">
        <el-input v-model="selectedNode.name" size="medium" v-show="showInput"
                  style="width: 300px" @blur="showInput = false"></el-input>
        <el-link v-show="!showInput" @click="showInput = true" style="font-size: medium">
          <i class="el-icon-edit" style="margin-right: 10px"></i>
          {{selectedNode.name}}
        </el-link>
      </div>
      <div class="node-config-content">
        <node-config/>
      </div>
    </el-drawer>
  </el-main>
</template>

<script>
import ProcessTree from './ProcessTree.vue'
import ProcessSide from './ProcessSide.vue'
import NodeConfig from './node/config/NodeConfig'
//import ProcessTree from '@/views/common/process/FormComponentConfig.vue'
import html2canvas from 'html2canvas';

export default {
  name: "ProcessDesign",
  components: {ProcessTree, NodeConfig, ProcessSide},
  data() {
    return {
      scale: 100,
      selected: {},
      showInput: false,
      showConfig: false
    }
  },
  computed:{
    selectedNode(){
      return this.$store.state.selectedNode
    },
    process(){
      return this.$store.state.design.process
    }
  },
  mounted() {

  },
  methods: {
    validate(){
      this.$refs["process-tree"].validateProcess()
    },
    nodeSelected(node){
      console.log('配置节点', node)
      this.showConfig = true
    },
    save() {
      console.log(JSON.stringify(this.process, null, 4), '保存json数据')
    },
    saveImg(){
      html2canvas(document.getElementsByClassName('_root')[0], {
        backgroundColor: '#f5f6f6'
      }).then(canvas => {
        // document.body.appendChild(canvas)
        const imgData = canvas.toDataURL('image/png')
        const a = document.createElement('a')
        a.download = 'flow图片'
        a.href = imgData
        a.click()
        a.remove()
      })
    }
  },
  watch:{
    selectedNode:{
      deep: true,
      handler(node){
        console.log("更新")
        this.$refs["process-tree"].nodeDomUpdate(node)
      }
    }
  }
}
</script>

<style lang="less" scoped>
.design {
  margin-top: 60px;
  display: flex;
  transform-origin: 50% 0px 0px;
}

.scale {
  z-index: 999;
  position: fixed;
  top: 40px;
  right: 40px;

  span {
    margin: 0 10px;
    font-size: 15px;
    color: #7a7a7a;
    width: 50px;
  }
}

.node-config-content{
  padding: 0 20px 20px;
}

/deep/ .el-drawer__body{
  overflow-y: auto;
}
</style>
