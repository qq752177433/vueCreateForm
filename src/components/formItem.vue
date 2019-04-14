<template>
  <div class="flexBox">
    <div class="formItem" v-for="(item,index) in data" :key="index" :style="`width:${item.width ? item.width : 100/item.length}%`">
      <form-item v-if="item.formData.length !== 0" :data="item.formData"/>
      <div v-if="item.content.length !== 0" class="content">
        <span v-for="(content,index) in  item.content">
          <span v-if="content.type === 'b1'">{{content.text}}</span>
          <el-radio v-if="content.type === 'b2'" :label="content.text" v-model="item.value">{{content.text}}</el-radio>
          <el-checkbox v-if="content.type === 'b3'" :label="content.text" v-model="item.values">{{content.text}}</el-checkbox>
        </span>
      </div>
    </div>
    <div class="clearFloat"></div>
  </div>
</template>


<script>
export default {
  name: 'formItem',
  components: {},
  props: {
    data: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {

    }
  },

  computed: {

  },
  methods: {
  },
}
</script>
<style scoped>
.flexBox{
  display: flex;
  flex-wrap:wrap;
  min-height: 100px;
  outline:#000 solid 1px;
}
.formItem{
  min-height: 100px;
  outline:#000 solid 1px;
}
.content{
  width: 100%;
  height: 100%;
  text-align: left;
}
.content > span{
  line-height: 100px;
}
.clearFloat{
  content: '';
  clear: both;
  display: block;
}
</style>
