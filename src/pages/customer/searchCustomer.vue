/**
* Created by ztt on 2018/5/13.
*/
<template>
    <div id="">
      <transition-group enter-active-class="animated slideInUp" leave-active-class="animated slideOutDown" :duration="800">
        <div :key="1" v-show="parentData.customerShow" style="z-index:100;background:white;width: 100%;height:100%;position: fixed;top: 0;left:0 ">
          <div :key="2" style="color: white;height:56px;background:#C20C0C;font-size: 18px;margin: 0 auto;width: 100%;text-align: center;line-height: 56px;">
            <span>客户信息</span>
            <div style="color:white;float: left;font-size: 25px;width: 56px;height: 20px;">
              <div @click="$router.push('/user/customerInfo')" :key="5" style="color: white" >
                <i :key="6" class="ion-ios-plus-empty"></i>
              </div>
            </div>
            <div :key="3" style="float: right;font-size: 25px;width: 56px;height: 20px;"  @click="parentData.customerShow=false" >
              <i :key="4" class="ion-ios-close-empty"></i>
            </div>
          </div>
          <div :key="14">
            <div :key="11" style="margin: 3px;background: #F2F2F2">
              <i :key="12" style="margin-left: 15px;margin-right: 5px;" class="ion-ios-search"></i>
              <input :key="13" style="height: 35px;background: #F2F2F2" type="text" placeholder="姓名/手机号"/>
            </div>
            <div :key="7" v-if="customerList.length>0||myData.pageNum==0">
              <van-list
                v-model="loading"
                :finished="finished"
                :offset=100
                @load="onLoad"
              >
                <div @click="choosedCustomer(index,item.customerName,item.id)" v-for="(item,index) in customerList">
                  <div style="width: 70%;height: 40px;padding-left: 15px;line-height: 40px;float: left">{{item.customerName}}</div>
                  <i v-if="showChoosed[index]" style="float: right;font-size: 20px;margin-right: 15px;" class="ion-checkmark-round"></i>
                  <div class="ok-model-border"></div>
                </div>
              </van-list>
              <div :key="8" v-if="finished&&myData.pageNum>1" style="color: #888888;text-align: center;padding: 20px;">
                到底了别滑了，真的没了.....
              </div>
            </div>
            <div :key="9" v-if="myData.pageNum!=0&&customerList.length==0&&finished==true"  style="color: #888888;text-align: center;padding: 20px;">
              没有您要找的客户......
            </div>
          </div>
        </div>
      </transition-group>
    </div>
</template>

<script>
  import {getCustomerList} from '@/service/getData.js';
  import { List } from 'vant';
  import Vue from 'vue'
  Vue.use(List);
    export default {
        mixins: [],     //混合
        components: {},//注册组件
        data() {         //数据
            return {
              loading: false,
              finished: false,
              customerList:[],
              showChoosed:[],
              myData:{paging:true,pageNum:0,limit:25},
            };
        },
        props:{
          parentData:{}
        },
        computed: {},  //计算属性
        created() {
        },   //创建
        mounted() {
          this.onLoad();
        },   //挂载
        methods: {
          choosedCustomer(index,customerName,customerId){
            for(var i=0;i<this.customerList.length;i++){
              this.showChoosed[i]=false;
            }
            this.showChoosed[index]=true;
            Vue.set(this.showChoosed,index,this.showChoosed[index]);
            this.parentData.customerName=customerName;
            this.parentData.customerId=customerId;
            this.parentData.customerShow=false;
          },
          onLoad() {//上划加载商品
            this.myData.pageNum++;
            getCustomerList(this.myData).then(
              response=>{
                for(var i=0;i<response.data.results.length;i++){
                  this.customerList.push(response.data.results[i]);
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
            this.customerList=[];
            this.myData.pageNum=0;
            this.finished=false;
            this.onLoad();
          }
        },   //方法
        watch: {
          // 'parentData.customerShow':function (newValue,oldValue) {
          //   if(newValue){
          //     this.onLoad();
          //   }
          // }
        }      //监听
    }
</script>

<style scoped>

</style>
