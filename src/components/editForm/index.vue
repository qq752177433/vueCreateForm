<template>
  <div class="rightSpace">
    <el-row>
      <el-col :span="22">
        <form-row v-if="data.type === 'l1'" :data="data"></form-row>
      </el-col>
    </el-row>
    <el-row>
    </el-row>
  </div>
</template>


<script>
import formRow from './formRow'
export default {
  components: {formRow},
  props: {
    data: {
      type: Object,
      default: () => {},
    },
    index: {
      type: Number,
      default: 1,
    },
  },
  data() {
    return {

    }
  },
  methods: {
    copyData (index) {
      this.$emit("handleCopy",index)
    },
    deleteData (index) {
      this.$emit("handleDelete",index)
    },
  }
}
</script>
<style scoped>
.rightSpace{
  width: 100%;
  border: 1px solid #dcdfe6;
  margin: 5px 10px;
}
.rightSpace .number{
  padding: 5px 8px;
  display: inline-block;
  border: 1px solid #dcdfe6;
  margin: 5px;
}
.title{
  margin: 5px;
}
.title span{
  margin: 0px 10px 0 0;
}
</style>
