<template>
  <div class="center-detail">
    <div class="movie-main">
      <p>标题党</p>
      <article>
        <span class="article-content" ref="articleContent">{{content}}</span>
        <span class="span-clr" ref="pGameModel">{{gameModel}}</span>
        <span>数量:</span>
        <input type="text" class="number-ipt" ref="numIpt" v-bind:value="numId" disabled />
      </article>
      <div class="join-shop-car">
        <span class="join-btn" v-on:click="joinCar()">加入购物车</span>
      </div>
    </div>
    <footerCom></footerCom>
  </div>
</template>
<script>
  import footerCom from '@/components/footerCom'
  export default{
    name:'centerDetail',
    data(){
      return{
        content:'',
        gameModel:'',
        numId:''
      }
    },
    components:{
      footerCom
    },
    methods:{
      //获取列表对应的详情信息
      getMovieDetail(){
        let _this = this;
        _this.$axios.get('https://easy-mock.com/mock/5ad85acd8ddd0450275c0dd4/myTest/movieDetail'+"?id="+_this.$route.query.id)
        .then(function(res){
          _this.content = res.data.data.list.content;
          _this.gameModel = res.data.data.list.gameModel;
          _this.numId = res.data.data.list.id;
        })
        .catch(function(res){
          console.log(res);
        });
      },
      //购物车,把要加入购物车的参数和数据存入localStorage中,本地存储
      joinCar(){
        let _this = this;
        let pContent = _this.$refs.articleContent.innerText;//获取dom元素使用$refs.(ref的值).(对应的方法)
        let pGameModel = _this.$refs.pGameModel.innerText;
        let pNum = _this.$refs.numIpt.value;
        let keyJson = {"pNum":pNum,"pContent":pContent,"pGameModel":pGameModel};
        let keyJsonString = JSON.stringify(keyJson);
        let keyName = 'keyId' + pNum;
        for(let i=0;i<localStorage.length;i++){
          if(localStorage.key[i] == keyName){
            localStorage.removeItem(keyName);
          }
        }
        localStorage.setItem("keyId"+pNum,keyJsonString);
      }
    },
    create(){

    },
    mounted(){
      this.getMovieDetail();
    }
  }
</script>
<style lang="less" scoped>
  .movie-main{
    padding:10px;
    font-size: 20px;
    p{
      text-align: center;
      font-size: 24px;
      font-weight: bold;
      padding:10px 0;
    }
  }
  .join-shop-car{
    width: 100%;
    height: 80px;
    text-align: right;
    border:1px solid #e8e8e8;
    margin: 25px 0 100px 0;
    .join-btn{
      color: #fff;
      display: inline-block;
      padding:10px 20px;
      font-size: 22px;
      border-radius: 5px;
      background-color: red;
      margin: 13px 20px 13px 0;
    }
  }
  .span-clr{
    color: red;
  }
  .number-ipt{
    background-color: #fff;
    border:1px solid #fff;
  }
</style>
