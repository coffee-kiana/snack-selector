<script setup>
import { ref,onMounted,onBeforeMount ,watch} from 'vue'
import axios from 'axios';

const contain = ref('')
const notContain = ref('')
const selectList = [
  { id: 1, label: '糖', props: 'sugar' },
  { id: 2, label: '牛奶', props: 'milk' },
  { id: 3, label: '辣', props: 'spic' }
]
let snackList=[]

// 定义获取数据的函数
const fetchData = async () => {
  try {
    const response = await axios.get('https://query-huaweinckfilter-fprqyqssrq.cn-hangzhou.fcapp.run');
    snackList= response.data;
    console.log(response,'response')
  } catch (error) {
    console.error('请求数据出错:', error);
  }
};

// 在组件加载前时获取数据
onBeforeMount(fetchData);
watch(fetchData)
</script>

<template>
  <div class="filterWrapper">
    <div class="filterItemWrapper">
      <div class="title">包含</div>
      <div class="radioList">
        <div class="radioItemWrapper" v-for="item in selectList" :key="item.props">
          <input type="radio" :id="item.props" :value="item.props" v-model="contain" />
          <label :for="item.props">{{ item.label }}</label>
        </div>
      </div>
    </div>
    <div class="filterItemWrapper">
      <div class="title">不包含</div>
      <div class="radioList">
        <div class="radioItemWrapper" v-for="item in selectList" :key="item.props">
          <input type="radio" :id="item.props" :value="item.props" v-model="notContain" />
          <label :for="item.props">{{ item.label }}</label>
        </div>
      </div>
    </div>
    <div class='btnGroup'>
      <button>确 定</button>
      <button>清 空</button>
    </div>
    <div class="imgWrapper">
      <div v-for="(item,index) in snackList" :index="index" :key="index">
        {{item.image_url}}
        <img :src='item.image_url'/>
      </div>
    </div>
  </div>
</template>
<style scoped>
.filterWrapper{
  display: flex;
  flex-direction: column;
  gap:20px;
  align-items: center;
}
.filterItemWrapper {
  width: 60%;
  padding: 20px;
  background-color: #f3f3f3;
  border-radius: 16px;
}
.title {
  font-size: 20px;
  line-height: 20px;
  color: #000;
  margin-bottom: 10px;
}
.radioList {
  display: flex;
  gap: 10px;
  flex: 1 1 1;
}
.radioItemWrapper {
  width: 30%;
  display: flex;
  gap: 5px;
  font-size: 16px;
  align-items: center;
}
.radioItemWrapper input{
  width:15px;
  height: 15px;
}
.btnGroup{
  display: flex;
  justify-content: space-around;
  gap: 40px;
}
.btnGroup button{
  border: 0;
  font-size: 16px;
  line-height: 16px;
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 500;
color:#fff;
  background-color: #11BC98;
}
.imgWrapper{
  width: 100vw;
  padding: 0 20px;
}
</style>
