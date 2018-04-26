<template>
  <div class="shopcar">
    <h1>我的购物车</h1>
    <div class="car-list">
      <ul class="car-ul">
        <li class="clearfix" v-for="item in productList">
          <div class="img-wd">
            <img src="../images/login/user-img.jpg">
          </div>
          <div class="product-detail">
            <p class="product-title">{{item.pGameModel}}</p>
            <p class="product-article">
              {{item.pContent}}
            </p>
            <div class="product-num">
              <span class="num-icon-lev">-</span>
              <input type="text" class="num-ipt" v-bind:value="item.pNum" />
              <span class="num-icon-add">+</span>
              <span class="del-product" v-on:click="removeItemFn(item.pNum)">删除</span>
            </div>
          </div>
        </li>
      </ul>
    </div>
    <footerCom></footerCom>
  </div>
</template>
<script>
  import footerCom from '@/components/footerCom'
  export default{
    data(){
      return{
        name:'shopcarMd',
        productList:[] //购物车订单
      }
    },
    components:{
      footerCom
    },
    computed:{

    },
    methods:{
      //获取localStorage存储的数据，转换为json对象
      getLocalStorageMsg(){
        let carArry = new Array();
        for(let i=0;i<localStorage.length;i++){
          let localStorageVal = localStorage.getItem(localStorage.key(i));
          let newKey = localStorage.key(i);
          if(newKey != 'keyId' && newKey.indexOf('keyId') >=0){
            let obj = JSON.parse(localStorageVal);
            carArry.push(obj);
          }
        }
        this.productList = (carArry);
      },
      //点击删除,则清除该条localStorage数据
      removeItemFn(keyVal){
        for(let j=0;j<localStorage.length;j++){
          if('keyId'+keyVal == localStorage.key(j)){
            localStorage.removeItem('keyId'+keyVal);
          }
        }
        this.$nextTick(function(){
          this.getLocalStorageMsg();
        })
      }
    },
    beforecreated(){

    },
    created(){
      //这里适合放不存在dom操作的部分请求
    },
    beforeMount(){

    },
    mounted(){
      //这部分适合放操作dom的请求
      this.getLocalStorageMsg();
    }
  }
</script>
<style lang="less" scoped>
  .clearfix:after{
    display: block;
    clear: both;
    visibility:hidden;
    content: '';
    height:'0';
  }
  .clearfix{
    zoom:1;
    overflow:hidden;
  }
  .shopcar{
    h1{
      color: #fff;
      font-size: 26px;
      text-align: center;
      padding: 20px 0;
      background-color: rgba(0,0,0,0.8);
    }
    li{
      position: relative;
      padding:20px 2%;
      border-bottom: 1px solid #ccc;
    }
    .img-wd{
      float: left;
      width: 20%;
      margin-top: 5px;
    }
    .car-ul img{
      width: 100%;
      border-radius: 50%;
      display: inline-block;
      vertical-align: middle;
      border:1px solid #ccc;
    }
    .product-detail{
      float: right;
      width: 75%;
      margin-left: 1%;
    }
    .product-detail{
      display: inline-block;
    }
    .product-title{
      font-weight: bold;
      font-size: 24px;
      height: 30px;
      margin-bottom: 10px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .product-article{
      font-size: 20px;
      height: 50px;
      display: -webkit-box;
      -webkit-box-orient:vertical;
      -webkit-line-clamp:2;
      overflow:hidden;
    }
    .product-num{
      font-size: 20px;
      text-align: left;
      margin-top: 10px;
    }
    .num-ipt{
      text-align: center;
      width: 100px;
    }
    .num-icon-lev,.num-icon-add{
      font-size: 22px;
      font-weight: bold;
      display: inline-block;
      padding:0 10px;
      background-color: #ccc;
      vertical-align: middle;
    }
    .del-product{
      position: absolute;
      bottom: 20px;
      right: 10px;
      display: inline-block;
      height: 30px;
      width: 80px;
      text-align: center;
      background-color: #ccc;
      border-radius: 5px;
    }
  }
</style>
