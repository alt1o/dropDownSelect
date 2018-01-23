<template>
  <div class="dropDownSelect">
    <div class="inputBox" @click="showDorpList">
      <span class="inputBox-item" v-for="(item,index) in chosen" :key="index">
        {{optionsList[item].name}}
        <div class="inputBox-item-close" @click="choose(item,$event)">
          <img src="../assets/close.png" alt="">
        </div>
      </span>
      <div  class="inputBox-addBtn">
        <img src="../assets/addBtn.png" alt="">
      </div>
    </div>
    <div class="dropBox" v-if="isShowDropBox">
      <div class="searchBox">
        <input type="text" placeholder="搜索" v-model="filterText">
        <img src="../assets/search.png" alt="">
      </div>
      <div class="optionsBox"  :style="{height:optionsListHeight}">
        <ul class="optionsList">
          <li v-for="(item, index) in showList" 
            :key="index" 
            @click="choose(index,$event)">
            <img :src="isSelect(index)">
            <div>{{item.name}}</div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
const KEYS = ["name", "value"];
const LEN = 10;
const CHOOSE_PIC_URL = [
  require('../assets/box.png'),//未选中的图案
  require('../assets/boxChosen.png')//选中的图案
];
export default {
  name: "dropDownSelect",
  props: {
    options: {
      validator: Array,
      required: true
    },
    selectedList:{
      validator:Array
    },
    optionsListHeight:{
      validator:String,
      default:"200px"
    }
  },
  data() {
    return {
      optionsList: [],
      chosen: [],
      isShowDropBox:false,
      filterText:''
    };
  },
  created(){
    this.optionsList = checkPropsOptions(this.options);
    this.chosen = checkPropsChosen(this.selectedList,this.optionsList.length);
  },
  watch:{
    options(value,oldValue){
      this.optionsList = checkPropsOptions(value);
      this.chosen = [];
    },
    chosen(value,oldValue){
      this.$emit("selectChange",value.map((item)=>{
        return this.optionsList[item];
      }))
    }
  },
  computed:{
    showList:function(){//显示出来的列表
      if(this.filterText === ''){
        return this.optionsList;
      }else{
        return this.optionsList.filter((item,index)=>{
          return item.name.indexOf(this.filterText) !== -1;
        })
      }
    }
  },
  methods:{
    isSelect:function(index){//判断选项是否被选择
      if(this.chosen.indexOf(index) !== -1){
        return CHOOSE_PIC_URL[1];
      }else{
        return CHOOSE_PIC_URL[0];
      }
    },
    choose:function(index,event){//控制选择
      event.stopPropagation();
      let ind = this.chosen.indexOf(index);
      if(ind === -1){
        this.chosen.push(index);
      }else{
        this.chosen.splice(ind,1);
      }
    },
    showDorpList:function(){//控制下拉列表的显示
      this.isShowDropBox = !this.isShowDropBox;
    }
  }
};
/**
 * 用于检测传入的options是否合法。
 * @param value 父组件传入的数据
 * 
 */
function checkPropsOptions(value) {
  let len = value.length;
  let cache;
  if (!(value instanceof Array) || len == 0) {
    return [];
  }
  for (let i = 0; i < len; i++) {
    if (!(value[i] instanceof Object)) {
      return [];
    }
    if (KEYS[0] in value[i]) {
      cache = value[i][KEYS[0]].toString();
      if (cache.length > LEN) {
        value[i][KEYS[0]]=cache.substr(0,10);
      }else{
        value[i][KEYS[0]]=cache;
      }
    }else{
      return [];
    }
    if(!(KEYS[1] in value[i])) {
      return [];
    }
  }
  return value;
}
/**
 * 用于检测传入的selectedList是否合法。
 * @param value 父组件传入的数据
 * @param maxlen 根据传入的options确定长度
 * 
 */
function checkPropsChosen(value,maxLen) {
  if(!(value instanceof Array)){
    return [];
  }
  for(let i=0; i < value.length; i++){
    value[i] = parseInt(value[i],10);
    if( isNaN(value[i]) ){
      return [];
    }
    if( value[i] >= maxLen){
      return [];
    }
  }
  return value;
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dropDownSelect {
  position: relative;
  font-size: 14px;
}
.inputBox {
  width: 100%;
  height: 36px;
  padding: 5px 5px;
  padding-right: 30px;
  border: 1px solid #c1c1c1;
  border-radius: 2px;
  box-sizing: border-box;
  position: relative;
  text-align: left;
  overflow: hidden;
  white-space: nowrap;
  background: #ffffff;
  cursor: pointer;
}
.inputBox .inputBox-item {
  position: relative;
  width: auto;
  height: 24px;
  display: inline-block;
  line-height: 24px;
  color: #ffffff;
  font-size: 14px;
  padding-left: 5px;
  padding-right: 25px;
  margin-left:5px;
  background: #2c72de;
  border-radius: 12px;
}
.inputBox .inputBox-item .inputBox-item-close {
  position: absolute;
  top: 0px;
  right: 0px;
  width: 24px;
  height: 24px;
}
.inputBox .inputBox-item .inputBox-item-close img {
  position: absolute;
  top: 6px;
  right: 6px;
  width: 12px;
  height: 12px;
}
.inputBox .inputBox-addBtn {
  position: absolute;
  top: 0px;
  right: 0px;
  width: 30px;
  height: 36px;
  background: #fff;
}
.inputBox .inputBox-addBtn img {
  position: absolute;
  top: 9px;
  right: 9px;
  width: 16px;
  height: 16px;
}

.dropBox {
  position: absolute;
  top: 35px;
  left: 0;
  width: 100%;
  background: #fff;
  z-index: 998;
}
.dropBox .searchBox {
  position: relative;
  width: 100%;
  height: 44px;
  background: #f5f5f5;
  padding: 7px 20px;
  box-sizing: border-box;
}
.dropBox .searchBox input {
  width: 100%;
  height: 30px;
  border: 1px solid #c1c1c1;
  border-radius: 15px;
  padding-left: 35px;
  box-sizing: border-box;
}
.dropBox .searchBox img {
  position: absolute;
  top: 14px;
  left: 30px;
  width: 16px;
  height: 16px;
}
.dropBox .optionsBox{
  width: 100%;
  height: 200px;
  overflow: auto;
}
.dropBox .optionsList {
  margin: 0 0;
  padding: 10px 30px;
  list-style: none;
  text-align: left;
}
.dropBox .optionsList li {
  position: relative;
  width: 100%;
  height: auto;
  padding: 10px 0;
  box-sizing: border-box;
  cursor: pointer;
}
.dropBox .optionsList li img {
  position: absolute;
  top: 11px;
  left: 0;
  width: 16px;
  height: 16px;
}
.dropBox .optionsList li div {
  padding-left: 30px;
  line-height: 19px;
}
</style>
