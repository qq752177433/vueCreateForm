<template>
  <el-row>
    <el-col :span="8" style="border:1px solid #dcdfe6">
      <div class="leftContent">
        <div class="leftSpace">
          <h3>基础表单</h3>
          <draggable
            :list="menu.layout"
            :group="{ name: 'layout', pull: 'clone', put: false }"
          >
          <div class="leftItem" v-for="item in menu.layout" @click="addSubject(item)">
            {{item.name}}
          </div>
          </draggable>
        </div>
        <div class="leftSpace">
          <h3>字段选择</h3>
          <draggable
            :list="menu.base"
            :group="{ name: 'base', pull: 'clone', put: false }"
            :clone="cloneDeep"
          >
          <div class="leftItem" v-for="item in menu.base">
            {{item.name}}
          </div>
          </draggable>
        </div>
      </div>
    </el-col>
    <el-col :span="16" style="">
      <div class="rightContent">
        <!-- <draggable element="div" v-model="data" :animation="300" group="people">
          <form-item v-for="(item,index) in data" :data="item" :index="index" @handleCopy="handleCopy" @handleDelete="handleDelete"></form-item>
        </draggable> -->
        <draggable element="div" v-model="data" :animation="300" group="layout">
          <edit-form v-for="(item,index) in data" :data="item" :index="index" @handleCopy="handleCopy" @handleDelete="handleDelete"></edit-form>
        </draggable>
        <!-- <a @click="showData">点击我展示最新数据</a> -->
      </div>
      <el-button @click="showData">预览（需要先添加行或列）</el-button>点击创建表格=》》》》添加行或列=》》》拖拽字段

      <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="80%">
        <div style="margin:10px 0" v-for="(item,index) in data">
          <form-item :data="item.formData"></form-item>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
        </span>
      </el-dialog>
    </el-col>
  </el-row>
</template>


<script>
import EditForm from './editForm'
import FormItem from './formItem'
import draggable from 'vuedraggable'
export default {
  components: {EditForm,draggable,FormItem},
  data() {
    return {
      data: [],
      dialogVisible: false,
      menu: {
        base: [
          {
            name: "文本型",
            type: "b1",
            group: "base",
          },
          {
            name: "单选型",
            type: "b2",
            group: "base",
          },
          {
            name: "多选型",
            type: "b3",
            group: "base",
            values: [],
          },
        ],
        layout: [
          {
            name: "点击创建表格",
            type: "l1",
            layout: 2,
            length: 1,
            content: [],
            values: [],
            formData: [],
            group: "layout",

          },
        ],
      },
    }
  },
  methods: {
    addSubject (item) {
      this.data.push(JSON.parse(JSON.stringify(item)))
    },
    showData () {
      console.log(this.data)
      this.dialogVisible = true
    },
    handleCopy (index) {
      this.data.push(JSON.parse(JSON.stringify(this.data[index])))
    },
    handleDelete (index) {
      this.data.splice(index,1)
    },
    cloneDeep (data) {
      return JSON.parse(JSON.stringify(data))
    },
  }
}
</script>
<style scoped>
.longer{
  width: 350px
}
.leftContent{
  width: 80%;
  margin: auto;
}
.leftSpace{
  overflow: hidden;
}
.leftContent .leftItem{
  border: 1px solid #dcdfe6;
  text-align: center;
  padding: 5px 15px;
  float: left;
  margin: 10px 15px;
  cursor: pointer;
}
.rightContent{
  min-height: 100px;
}
</style>
