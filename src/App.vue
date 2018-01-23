<template>
  <div id="app">
    <drop-down-select 
      :options="options" 
      :selectedList="selectedList" 
      v-on:selectChange="selectChange"
      optionsListHeight="120px"/>
    <div class="content">
      <button @click="changeData">改变传入的数据</button>
      <p>测试一下会不会被下方内容遮挡</p>
      <p>测试一下会不会被下方内容遮挡</p>
      <p>参数：</p>
      <p>options:必须传入，用于渲染下拉列表数据，数据格式为 [{name:"name",value:"value"},{name:"name",value:"value"}]，其中name用于显示，value用于附带的其他数据</p>
      <p>selectedList:表示默认已经被选的选项在options中序号，格式为：[2,3]表示默认选中第2，3项</p>
      <p>selectChange:方法会在用户改变选项后触发</p>
      <p>optionsListHeight:用于设置下拉列表的高度，默认为200px</p>
    </div>
  </div>
</template>

<script>
import dropDownSelect from './components/dropDownSelect'

export default {
  name: 'App',
  components: {
    dropDownSelect
  },
  data(){
    return {
      options:[
        {name:'one',value:'1'},
        {name:'two',value:'2'},
        {name:'three',value:'3'},
        {name:'four',value:'4'},
      ],
      selectedList:[2,3]
    }
  },
  methods:{
    selectChange:function(selected){
      console.log('用户已选择：',selected);
    },
    changeData:function(){
      this.options=[
        {name:"项目一",value:"3"},
        {name:"项目二",value:"4"}
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.content{
  position: absolute;
  top:100px;
  left:0;
  width: 100%;
  z-index: 997;
  text-align: left;
}
</style>
