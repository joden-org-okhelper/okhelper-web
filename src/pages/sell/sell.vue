/**
* Created by ztt on 2018/4/17.
*/
<template>
    <div id="" style="height: 100%;">
      <div  style="color:white;background: #C20C0C;font-size:16px;line-height:56px;height: 56px;width: 100%;text-align: center;">
        <div style="display: block;font-size: 20px;">销售</div>
        <!-- <div style="display: block;float: right;margin-right: 20px;margin-top: -55px;" @click="show=!show">收款码</div> -->
      </div>
      <div class="ok-border"></div>
      <router-link to="/sell/sellTable" class="ok-sellOrStore-model">
        <div class="ok-sellOrStore-box" style="border-right:0.5px solid #F2F2F2;">
          <div class="ok-sellOrStore-icon">
            <i class="ion-ios-pricetags"></i>
          </div>
          <div class="ok-sellOrStore-content">
            <div>销售单</div>
            <div class="ok-sellOrStore-subcontent">卖货、出库</div>
          </div>
        </div>
      </router-link>
      <router-link to="/product" class="ok-sellOrStore-model">
        <div class="ok-sellOrStore-box">
          <div class="ok-sellOrStore-icon">
            <i class="ion-ios-star"></i>
          </div>
          <div class="ok-sellOrStore-content">
            <div>商品展示</div>
            <div class="ok-sellOrStore-subcontent">商品单价、照片</div>
          </div>
        </div>
      </router-link>
      <div class="ok-model-border"></div>
      <!-- <router-link to="/500" class="ok-sellOrStore-model">
        <div class="ok-sellOrStore-box" style="border-right:0.5px solid #F2F2F2;">
          <div class="ok-sellOrStore-icon">
            <i class="ion-reply"></i>
          </div>
          <div class="ok-sellOrStore-content">
            <div>销售退货单</div>
            <div class="ok-sellOrStore-subcontent">退货、换货</div>
          </div>
        </div>
      </router-link> -->
      <router-link to="/sell/sellHistory" class="ok-sellOrStore-model">
        <div class="ok-sellOrStore-box" style="border-right:0.5px solid #F2F2F2;">
          <div class="ok-sellOrStore-icon">
            <i class="ion-ios-calendar-outline"></i>
          </div>
          <div class="ok-sellOrStore-content">
            <div>销售历史</div>
            <div class="ok-sellOrStore-subcontent">销售历史</div>
          </div>
        </div>
      </router-link>
      <div style="clear: both" class="ok-border"></div>
      <!--<router-link to="/checkstand" class="ok-sellOrStore-model">-->
        <!--<div class="ok-sellOrStore-box" style="border-right:0.5px solid #F2F2F2;">-->
          <!--<div class="ok-sellOrStore-icon">-->
            <!--<van-icon name="points" />-->
          <!--</div>-->
          <!--<div class="ok-sellOrStore-content">-->
            <!--<div>收银台</div>-->
            <!--<div class="ok-sellOrStore-subcontent">销售历史</div>-->
          <!--</div>-->
        <!--</div>-->
      <!--</router-link>-->
      <!--<div class="ok-model-border"></div>-->
      <ok-footer></ok-footer>
      <transition-group enter-active-class="animated slideInUp" leave-active-class="animated slideOutDown">
        <div :key="1" v-show="show" style="z-index:100;background:#C20C0C;width: 100%;height: 100%;position: absolute;top: 0px; ">
          <div :key="2" style="color: white;font-size: 18px;margin: 0 auto;width: 50%;text-align: center;margin-top: 20px;">收款二维码</div>
          <div :key="3" style="height: 50px;width: 50px;line-height:25px;margin-top:-25px;text-align:center;float:right;margin-right:20px;color: white;font-size: 25px;">
            <i :key="4" @click="show=!show" class="ion-ios-close-empty"></i>
          </div>
          <div :key="5" style="width: 80%;margin: 0 auto;height: 320px;margin-top:70px;background: white;padding-top: 10px;">
            <div :key="6" style="width: 80%;height: 300px;margin: 0 auto;">
              <div :key="7" style="height: 70%;width: 100%;">
                <img v-if="alipayIsActive" :key="8" width="100%" height="110%" src="@/assets/icon/alipay.jpg"/>
                <img v-if="chatpayIsActive" :key="8" width="100%" height="110%" src="@/assets/icon/ok-icon-red.png"/>
              </div>
              <div :key="9" style="height: 30%;width: 100%;">
                <div :key="10" @click="payIsActive(1)" style="display:block;float:left;width: 50%;font-size: 30px;color: #108ee9;text-align: center;padding-top: 10px;">
                  <van-icon name="alipay" />
                  <div :key="11"  :class="{alipayActive:alipayIsActive}" style="font-size: 12px;color: #888888;">支付宝</div>

                </div>
                <div :key="12" @click="payIsActive(2)" style="display:block;float:left;width: 50%;font-size: 30px;text-align: center;padding-top: 10px;color: #439057;">
                  <van-icon name="wechat" />
                  <div :key="13"  :class="{chatpayActive:chatpayIsActive}" style="font-size: 12px;color: #888888;">微信</div>
                </div>
              </div>
            </div>
            <div :key="14" style="margin-top:20px;font-size: 10px;color: white;">
              <i :key="15" class="ion-ios-information-outline"></i>
              上图为您店铺的收款二维码，客户用支付宝或微信扫描此二维码付款

              <!--<van-uploader :after-read="onRead" accept="image/*" multiple>-->
                <!--<div :key="16"  style="color: yellow">点击【上传二维码】</div>-->
              <!--</van-uploader>-->
            </div>
          </div>
        </div>
      </transition-group>
    </div>
</template>

<script>
  import Vue from 'vue'
  import { Icon } from 'vant';
  import {upLoadPayImgs} from '@/service/getData.js'
  import { Uploader } from 'vant';
  import { Toast } from 'vant';
  Vue.use(Icon).use(Uploader);
  const Footer = resolve => require(['@/components/footer/footer'], resolve);
    export default {
        mixins: [],     //混合
        components: {
          'ok-footer':Footer
        },//注册组件
        data() {         //数据
            return {
              show:false,
              alipayIsActive:true,
              chatpayIsActive:false
            };
        },
        computed: {},  //计算属性
        created() {
        },   //创建
        mounted() {
        },   //挂载
        methods: {
          payIsActive(n){
            switch (n){
              case 1:this.alipayIsActive=true;this.chatpayIsActive=false;break;
              case 2:this.alipayIsActive=false;this.chatpayIsActive=true;break;
              default:this.alipayIsActive=true;this.chatpayIsActive=false;
            }
          },
          onRead(file) {
            let formData = new FormData();
              formData.append('file',file.file);
              upLoadGoodsImgs(formData).then(
                response=>{
                  console.log(response.data.url);
                  Toast({
                    position: 'bottom',
                    message: '图片上传成功'
                  });
                },error=>{
                  console.log(error.response.msg);
                  Toast(error.response.msg);
                }
              )
          }
        },   //方法
        watch: {}      //监听
    }
</script>

<style scoped>
  .alipayActive{
    color: #108ee9!important;
    font-size: 16px!important;
    font-weight: bolder!important;
  }
  .chatpayActive{
    color: #439057!important;
    font-size: 16px!important;
    font-weight: bolder!important;
  }
</style>
