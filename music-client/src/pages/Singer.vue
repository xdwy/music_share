<template>
  <div class="singer">
    <ul class="singer-header">
      <li v-for="(item,index) in singerStyle" :key="index" @click="handleChangeView(item)"
          :class="{active:item.name==activeName}">
        {{item.name}}
      </li>
    </ul>
    <content-list :contentList="data"></content-list>
    <div class="pagination">
      <el-pagination @current-change="handleCurrentChange" background
                     layout="total,prev,pager,next"
                     :current-page="currentPage"
                     :page-size="pageSize"
                     :total="albumDatas.length">
      </el-pagination>
    </div>
  </div>
</template>
<script>
import ContentList from '../components/ContentList';
import {getAllSinger,getSingerOfSex} from '../api/index';
import {mixin} from "../mixins";
import {singerStyle} from '../assets/data/singer';

export default {
  name: 'singer',
  mixins: [mixin],
  components:{
    ContentList
  },
  data(){
    return{
      albumDatas: [],   //歌手数据
      pageSize:15,       //页面大小，一页有15条数据
      currentPage:1,    //当前页，默认第一页
      singerStyle:[],     //风格
      activeName:'',    //当前高亮显示的风格
    }
  },
  computed:{
    //计算当前表格中的数据
    data(){
      return this.albumDatas.slice((this.currentPage-1)*this.pageSize,(this.currentPage)*this.pageSize)
    }
  },
  mounted(){
    this.activeName='全部歌手';
    this.singerStyle=singerStyle;
    this.getSingerList();
  },
  methods:{
    getSingerList(){
      getAllSinger()
        .then(res =>{
          this.albumDatas = res;
        })
    },
    //获取当前页
    handleCurrentChange(val){
      this.currentPage=val;
    },
    //根据性别显示对应的歌手
    handleChangeView(item){
      this.activeName=item.name;
      this.albumDatas=[];
      if(name=='全部歌手'){
        this.getSingerList();
      }else{
        this.goSingerOfSex(item.type);
      }
    },
    //根据性别查询对应的歌手
    goSingerOfSex(type){
      getSingerOfSex(type)
        .then(res =>{
          this.currentPage=1;
          this.albumDatas = res;
        })
    },
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/singer.scss';
</style>
