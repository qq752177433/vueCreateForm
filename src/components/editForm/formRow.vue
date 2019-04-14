<template>
  <div class="row">
    <div style="min-height:100px">
      <draggable class="drt" v-if="data.formData.length === 0" element="div" v-model="data.content" :animation="100" group="base">
        <div v-for="(item,index) in data.content" :key="index" :style="`width:100%`">
          <div class="content" v-if="item.type === 'b1'">
            <el-input style="min-width:100px" size="mini" v-model="item.text" placeholder="输入文本"></el-input>
          </div>
          <div class="content" v-if="item.type === 'b2'">
            <el-radio v-model="data.value" :label="item.text">
              <el-input style="width:200px;border:none;text-align:center" size="mini" placeholder="请输入选项" v-model="item.text"></el-input>
            </el-radio>
          </div>
          <div class="content" v-if="item.type === 'b3'">
            <el-checkbox v-model="data.values" :label="item.text">
              <el-input style="width:200px;border:none;text-align:center" size="mini" placeholder="请输入选项" v-model="item.text"></el-input>
            </el-checkbox>
          </div>
        </div>
        <div class="clearFloat"/>
      </draggable>
      <div class="flexBox" v-else>
        <div v-if="data.content.length === 0" class="layoutDiv" v-for="(item,index) in data.formData" :key="index" :style="`width:${100/item.length}%`">
          <itemCell :data="item"></itemCell>
        </div>
        <div class="clearFloat"/>
      </div>
    </div>
    <div class="handle">
      <el-button v-if="data.content.length === 0" type="text" @click="addColumn">添加列</el-button>
      <el-button v-if="data.content.length === 0" type="text" @click="addRow">添加行</el-button>
      <el-button type="text" @click="handleDelete">清空</el-button>
      <el-button type="text" v-if="data.layout !== 2">宽度<input style="width:50px;margin-left:5px" type="number" v-model="data.width" max="100" min="0"/></el-button>
    </div>
  </div>
</template>


<script>
import draggable from 'vuedraggable'
export default {
  name: 'itemCell',
  components: {draggable},
  props: {
    data: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      formData: []
    }
  },
  created () {
    // this.data.formData ? "" : this.data.formData = []
  },
  computed: {
    divWidth () {
      return 100/this.data.formData.filter(item=>item.layout === 1).length
    },
    borderColor () {
      return window.data ===  this.data ? "red" : "#000"
    }
  },
  methods: {
    addColumn () {
      let newArr = this.data.formData||[]
      if(newArr.length === 0){
        newArr.push({
          type:'l1',
          layout: 1,
          name:'列',
          length: 2,
          formData: [],
          content: [],
          values: [],
        },{
          type:'l1',
          layout: 1,
          name:'列',
          length: 2,
          formData: [],
          content: [],
          values: [],
        })
      }else{
        let initNumber = 1
        let getLength = (index)=>{
          if(index>=0 && newArr[index].layout == 1){
            initNumber ++
            getLength(index-1)
          }
        }
        let changeLength =  (number) =>{
          newArr.push({
            type:'l1',
            layout: 1,
            name:'列',
            length: 2,
            formData: [],
            content: [],
            values: [],
          })
          for (let i = 0;i<number;i++){
            newArr[i+newArr.length-number].length = number
          }
        }
        getLength(newArr.length-1)
        newArr[newArr.length-1].layout != 1 ? newArr.push({
          type:'l1',
          layout: 1,
          name:'列',
          length: 2,
          formData: [],
          content: [],
          values: [],
        },{
          type:'l1',
          layout: 1,
          name:'列',
          length: 2,
          formData: [],
          content: [],
          values: [],
        }) : changeLength(initNumber)
      }


      this.$set(this.data,'formData',newArr)
    },
    addRow () {
      let newArr = this.data.formData||[]
      newArr.push({
        type:'l1',
        layout: 2,
        name:'行',
        length: 1,
        formData: [],
        content: [],
        values: [],
      })
      if(newArr.length === 1){
        newArr.push({
          type:'l1',
          layout: 2,
          name:'行',
          length: 1,
          formData: [],
          content: [],
          values: [],
        })
      }
      this.$set(this.data,'formData',newArr)
    },
    handleDelete () {
      this.$set(this.data,'formData',[])
      this.$set(this.data,'content',[])
    },
    current () {
      console.log(this.data);
      this.$set(this.data,'confirm',true)
    },
    showDivdiv (number) {
      if(this.data.formData[number].layout === 1){
        let length = 0

        let getLength = (index) => {
          if(index>0){
            this.data.formData[index].layout === 1 ? length++ : ""
            getLength(index-1)
          }
        }
        getLength(number)
        console.log(length)
        return 100/length
      }else{
        return 100
      }

    },
  }
}
</script>
<style scoped>
.flexBox{
  display: flex;
  flex-wrap:wrap;
  box-sizing: border-box;
  outline: 1px solid #000;
}
.row{
  min-height: 100px;
  outline:#000 solid 1px;
  box-sizing: border-box;
  position: relative;
}
.row *{
  box-sizing: border-box;
}
.row span{
  margin: 0px 10px 0 0;
}
.mg10{
  margin: 10px 0;
}
.textIn{
  border: none;
  width: 100px;
}
.curP{
  cursor: pointer;
}
.layoutDiv{
  /* outline:#000 solid 1px; */
  min-height: 100px;
}
.clearFloat{
  content: '';
  clear: both;
  display: block;
}
.drt{
 min-height: 100px;
}
.content{
  padding: 5px;
  text-align: center;
}
</style>
