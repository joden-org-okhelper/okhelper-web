
/**
* Created by lulu on 2018/5/11.
*/


<template>
  <div id="">
    <div class="back-bar">
      <span @click="$router.back()" style="color: white" class="back-bar-backBtn">&lt;&nbsp;返回
      </span>
      <div class="back-bar-name">
      库存展示
      </div>
      <div class="back-bar-cancelBtn">
        <router-link to="/warehouse/purchaseOrder" style="display:block;float:left;width: 25px;height: 25px;font-size: 25px;color: white;font-weight: bolder;">
          <i class="ion-ios-plus-empty"></i>
        </router-link>
        <router-link to="/product/SearchProduct" style="margin-left:8px;display:block;float:left;width: 25px;height: 25px;font-size: 25px;color: white;font-weight: bolder;">
          <i class="ion-ios-search"></i>
        </router-link>
      </div>
    </div>
    <div style="margin-top:56px;height: 37px;width: 100%;">
      <div style="width: 25%;height: 37px;display: block;float: left" @click="arrowsShow(0)">
        <div style="width: 80%;margin: 0 auto;height: 37px;line-height: 37px;text-align: center;">
          <i v-if="arrows_show[0]" class="ion-arrow-down-c"></i>
          <span>默认</span>
        </div>
      </div>
      <div style="width: 25%;height: 37px;display: block;float: left" @click="arrowsShow(1)">
        <div style="width: 80%;margin: 0 auto;height: 37px;line-height: 37px;text-align: center;">
          <i v-if="arrows_show[1]" class="ion-arrow-down-c"></i>
          <span>价格</span>
        </div>
      </div>
      <div style="width: 25%;height: 37px;display: block;float: left" @click="arrowsShow(2)">
        <div style="width: 80%;margin: 0 auto;height: 37px;line-height: 37px;text-align: center;">
          <i v-if="arrows_show[2]" class="ion-arrow-down-c"></i>
          <span>销量</span>
        </div>
      </div>
      <div style="width: 25%;height: 37px;display: block;float: left" @click="arrowsShow(3)">
        <div style="width: 80%;margin: 0 auto;height: 37px;line-height: 37px;text-align: center;">
          <i v-if="arrows_show[3]" class="ion-arrow-down-c"></i>
          <span>库存</span>
        </div>
      </div>
    </div>
    <div class="ok-model-border"></div>
    <div v-if="productList.length>0||myData.pageNum==0">
      <van-list
        v-model="loading"
        :finished="finished"
        :offset=10
        @load="onLoad"
      >
        <div v-for="(item,index) in productList">
          <ok-product
            :main-img="item.mainImg"
            :product-name="item.productName"
            :cate-name="item.cateName"
            :discounts="item.youhui"
            :retail-price="item.retailPrice"
            :storage-price="item.storagePrice"
            :createTime="item.createTime"
            :Id="item.id"
            :index="index"
            :salesStock="item.salesStock"
            @addProduct="addProduct"
          />
        </div>
      </van-list>
      <div v-if="finished&&myData.pageNum>1" style="color: #888888;text-align: center;padding: 20px;">
        到底了别滑了，真的没了.....
      </div>
    </div>
    <div v-else  style="color: #888888;text-align: center;padding: 20px;">
      没有您要找的商品......
    </div>
    <div style="height: 30px;width: 100%;"></div>
    <!--<ok-category-->
      <!--:parentData="parentData"-->
      <!--@getChoosedCategoryId="getChoosedCategoryId"-->
    <!--&gt;</ok-category>-->

  </div>
</template>

<script>
  import Vue from 'vue'
  import { Collapse, CollapseItem } from 'vant';
  import WarehouseModel from '@/components/common/warehouseModel';
  import Category from "../category/category";
  import {getProductList} from '@/service/getData';
  import { List } from 'vant';
  Vue.use(List);
  Vue.use(Collapse).use(CollapseItem);
  export default {
    mixins: [],     //混合
    components: {
      'ok-category':Category,
      'ok-product':WarehouseModel
    },//注册组件
    data() {         //数据
      return {
        loading: false,
        finished: false,
        myData:{paging:true,pageNum:0,limit:6,categoryId:0,orderBy:'create_time desc'},
        // categoryName: 'categoryName', // 显示菜单名称的属性
        // parentData:{categoryShow:false,choosedCategoryName:'全部分类',plusShow:false},
        totalCount:0,
        arrows_show:[true,false,false,false],
        //productChoosedList:[],
        productList:[]
      };
    },
    computed: {},  //计算属性
    created() {
    },   //创建
    mounted() {
    },   //挂载
    methods: {
      onLoad() {//上划加载商品
        this.myData.pageNum++;
        getProductList(this.myData).then(
          response=>{
            for(var i=0;i<response.data.results.length;i++){
              this.productList.push(response.data.results[i]);
            }
            this.loading=false;
            if (response.data.lastPage) {
              this.finished = true;
            }
          },error=>{
            this.loading=false;
            this.finished = true;
            console.log(error.response.msg);
          }
        );
      },
      reLoad(){
        this.productList=[];
        this.myData.pageNum=0;
        this.finished=false;
        this.onLoad();
      },
      // getChoosedCategoryId(categoryItem){//获取从子组件来的分类id
      //   this.parentData.choosedCategoryName=categoryItem.categoryName;
      //   this.myData.categoryId=categoryItem.id;
      //   this.myData.orderBy='create_time desc';
      //   this.reLoad();
      // },
      arrowsShow(n) {
        for(var i=0;i<this.arrows_show.length;i++){
          this.arrows_show[i]=false;
        }
        switch (n){
          case 0:
            this.arrows_show[0]=true;
            Vue.set(this.arrows_show,0,this.arrows_show[0]);
            this.myData.orderBy='create_time desc';
            this.reLoad();
            break;
          case 1:
            this.arrows_show[1]=true;
            Vue.set(this.arrows_show,1,this.arrows_show[1]);
            this.myData.orderBy='retail_price desc';
            this.reLoad();
            break;
          case 2:
            this.arrows_show[2]=true;
            Vue.set(this.arrows_show,2,this.arrows_show[2]);
            this.myData.orderBy='retail_price desc';
            this.reLoad();
            break;
          case 3:
            this.arrows_show[3]=true;
            Vue.set(this.arrows_show,3,this.arrows_show[3]);
            this.myData.orderBy='sales_stock desc';
            this.reLoad();
            break;
          default :

        }
      },
      addProduct(goodsId,isActive){
        console.log(goodsId);
        if(isActive){
          this.productChoosedList.push(goodsId);
        }else {
          this.productChoosedList.remove(goodsId);
        }
      },
      // toCart(){
      //   this.$router.push({path:'/cart',query:{productChoosedList:this.productChoosedList}})
      // }
    },   //方法
    watch: {}      //监听
  }
</script>

<style scoped>

</style>
