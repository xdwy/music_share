<template>
  <div class="upload">
    <p class="title">修改头像</p>
    <hr/>
    <div class="section">
      <el-upload drag :action="uploadUrl()" :show-file-lost="false" :on-success="handleAvatorSuccess"
                 :before-upload="beforeAvatorUpload">
        <i class="el-icon-upload"></i>
        <div>
          将文件拖到此处，或<span style="color:blue">修改头像</span>
        </div>
        <div slot="tip">&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp
          只能上传jpg/png文件且不能超过10MB</div>
      </el-upload>
    </div>
  </div>
</template>

<script>
import {mapGetters} from 'vuex';
import {mixin} from '../mixins'
import {updateUserMsg, getUserOfId} from '../api/index'

export default {
  name: "upload",
  mixins:[mixin],
  computed:{
    ...mapGetters([
      'userId'
    ])
  },
  methods:{
    //上传地址
    uploadUrl(){
      return `${this.$store.state.configure.HOST}/consumer/updateConsumerPic?id=${this.userId}`;
    },
    //上传成功
    handleAvatorSuccess(res,file){
      if(res.code == 1){
        //上传成功
        this.$store.commit('setAvator',res.avator);
        this.notify('修改成功','success');
      }else{
        this.notify('修改失败','error');
      }
    },
    //上传前
    beforeAvatorUpload(file){
      const isJPG = file.type =='image/jpeg';
      const isLt10M = file.size / 1024 / 1024;
      if(!isJPG){
        this.notify('上传的图片只能是JPG格式','error');
        return false;
      }
      if(!isLt10M){
        this.notify('上传的图片大小不能超过10M','error');
        return false;
      }
    },
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/upload.scss";
</style>
