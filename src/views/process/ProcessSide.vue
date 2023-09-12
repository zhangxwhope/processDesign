<template>
  <div class="process-side">
    <div class="node-select">
      <div>
        <i class="el-icon-message" style="color:rgb(255, 148, 62);"></i>
        <span>请求节点</span>
      </div>
      <div>
        <i class="el-icon-date" style="color:rgb(50, 150, 250);"></i>
        <span>日期组件</span>
      </div>
      <div>
        <i class="el-icon-c-scale-to-original" style="color:rgb(21, 188, 131);"></i>
        <span>动态脚本</span>
      </div>
      <div>
        <i class="el-icon-help" style="color:#718dff;"></i>
        <span>字符串组件</span>
      </div>
      <div>
        <i class="el-icon-folder-delete" style="color:#f25643;"></i>
        <span>xml组件</span>
      </div>
      <div>
        <i class="el-icon-document" style="color:#c9bc0c;"></i>
        <span>json组件</span>
      </div>
      <div>
        <i class="el-icon-share" style="color:#3c2fe3;"></i>
        <span>条件分支</span>
      </div>
      <div>
        <i class="el-icon-share" style="color:#3c2fe3;"></i>
        <span>并行分支</span>
      </div>
      <div>
        <i class="el-icon-position" style="color:#f314e0;"></i>
        <span>变量组件</span>
      </div>
      <div>
        <i class="el-icon-refresh" style="color:#9896a7;"></i>
        <span>循环组件</span>
      </div>
      <div>
        <i class="el-icon-s-operation" style="color:#85d707;"></i>
        <span>数据转换</span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ProcessSide'
}
</script>

<style lang="less" scoped>
.process-side{
  width: 200px;
  height: 300px;
  border: 1px solid #ddd;
  overflow: auto;
  .node-select{
    div{
      display: inline-block;
      margin: 5px 5px;
      cursor: pointer;
      padding: 10px 15px;
      border: 1px solid #F8F9F9;
      background-color: #F8F9F9;
      border-radius: 10px;
      width: 130px;
      position: relative;
      span{
        position: absolute;
        left: 65px;
        top: 18px;
      }
      &:hover{
        background-color: #fff;
        box-shadow: 0 0 8px 2px #d6d6d6;
      }
      i{
        font-size: 25px;
        padding: 5px;
        border: 1px solid #dedfdf;
        border-radius: 14px;
      }
    }
  }
}
</style>