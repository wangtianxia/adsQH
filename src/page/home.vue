<template>
    <div class="home">
        <section v-if="loadingIndex>=10" class="homeBg">
            <img class="bg9Img" src="static/bg9.png" alt="">
            <section class="videoContent">
                <section class="bg3">
                    <img src="static/bg3.png" alt="">
                    <video id="myVideo" v-show="showVideo" src="https://adcdn.like000.com/adcdn/qhVideo.mp4"
                     controls="controls" webkit-playsinline="true" playsinline="true" ></video>
                </section>
                <img class="bg4" v-show="!showVideo"  @click="playVideo" src="static/bg4.png" alt="">
                <img class="bg2" src="static/bg2.png" alt="">
            </section>

            <section class="bg5Content">
                <img class="bg5" :src="srcBg5" alt="">
                <section class="bg5Word">当前已有<span>{{energy}}</span>人为她集气</section>
            </section>

            <img class="bg6" src="static/bg6.png" @click="bg7Click" alt="">
            <img class="bg7" src="static/bg7.png" alt="">

        </section>
        <img class="bg8" src="static/bg8.png" @click="bg8Click" alt="">
        <alert v-if="showAlert" @closeAlert="closeAlert"></alert>
        <transition  enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
            <div class="fadeDiv" v-if="isFade">
                {{words}}
            </div>
        </transition>
    </div>
</template>
<script>
    import Ajax from '../config/ajax'
    import alert from '../components/alert.vue'
    import env from '../config/env';
    import DataManager from '../dataManager/data';
    import MD5 from '../config/md5'

    export default {
        data() {
            return {
               loadingIndex:0,
               showVideo:false,
               isShowAlert:false,
               showAlert:false,
               isFade:false,
               energy:0,
               words:"",
               srcBg5:'static/s/s1.png'
            }
        },

        mounted(){
            for(let i=0;i<9;i++){
                let img = new Image();
                img.src = `static/bg${i}.png`;
                img.onload = ()=>{
                    this.loadingIndex+=1;
                }
            }
            let img = new Image();
            img.src = `static/bg0.jpg`;
            img.onload = ()=>{
                this.loadingIndex+=1;
            }

            let img2 = new Image();
            img2.src = `static/alert.jpg`;
            img2.onload = ()=>{
                this.loadingIndex+=1;
            }
            this.initData();

            for(let j=1;j<=56;j++){
                let img2 = new Image();
                img2.src = `static/s/s${j}.png`;
                img2.onload = ()=>{
                }
            }
          let data = {
            user_id: DataManager.user_id,
            group_openid: DataManager.group_openid,
            bid: DataManager.bid,
            type:6,
            ad_id: DataManager.ad_id
          };
          data.sign = MD5.makeSign(data);
          new Ajax().sendJSON(
            "https://sf.xintiao100.com/auBehavior",
            data,
            res => {
            }
          );
        },

        methods:{
            initData(){
                new Ajax().sendJSON(env+'/energy',{
                    user_id:DataManager.user_id,
                    bid:DataManager.bid,
                    group_openid:DataManager.group_openid,
                    sign:MD5.makeSign({
                        user_id:DataManager.user_id,
                        bid:DataManager.bid,
                        group_openid:DataManager.group_openid
                    })
                },(res)=>{
                    if(res['st'] == 1){
                        this.energy = res['data']['energy']
                    }
                });
            },
            // 动画
            special(){
                let index = 0;
                if(this.intervale){
                    return ;
                }
                this.intervale = setInterval(()=>{
                    index+=1;
                    this.srcBg5 = 'static/s/s'+index+'.png';
                    if(index>=56){
                         this.srcBg5 = 'static/s/s1.png';
                        clearInterval(this.intervale);
                        this.intervale = null;
                    }
                },30)
            },

//            bg6Click(){
//                if(this.isShowAlert){
//                    this.showAlert = true;
//                }
//                new Ajax().sendJSON(env+'/auBehavior', {
//                    user_id:DataManager.user_id,
//                    bid:DataManager.bid,
//                    group_openid:DataManager.group_openid,
//                    type:11,
//                    ad_id:DataManager.ad_id,
//                    sign:MD5.makeSign({
//                        user_id:DataManager.user_id,
//                        bid:DataManager.bid,
//                        group_openid:DataManager.group_openid,
//                        type:11,
//                        ad_id:DataManager.ad_id
//                    })
//                }, (res)=>{
//                    if(res['st'] == 1){
//                        this.initData();
//                        this.isFade = true;
//                        this.words = '集气成功'
//                        setTimeout(()=>{
//                            this.isFade = false
//                        }, 1500);
//                        this.special();
//                    }
//                    if(res['st'] == 2){
//                        this.isFade = true;
//                        this.words = '今天已参加集气'
//                        setTimeout(()=>{
//                            this.isFade = false
//                        }, 1500);
//                    }
//                })
//
//            },

            bg7Click(){
              if(this.isShowAlert){
                this.showAlert = true;
              }
                new Ajax().sendJSON(env+'/auBehavior', {
                    user_id:DataManager.user_id,
                    bid:DataManager.bid,
                    group_openid:DataManager.group_openid,
                    type:12,
                    ad_id:DataManager.ad_id,
                    sign:MD5.makeSign({
                        user_id:DataManager.user_id,
                        bid:DataManager.bid,
                        group_openid:DataManager.group_openid,
                        type:12,
                        ad_id:DataManager.ad_id
                    })
                }, (res)=>{
                    if(res['st'] == 1){
                        this.initData();
                        this.isFade = true;
                        this.words = '背包礼物+1,集气成功';
                        setTimeout(()=>{
                            this.isFade = false
                        }, 1500);
                        this.special();
                    }
                    if(res['st'] == 2){
                        this.isFade = true;
                        this.words = '今天已参加集气'
                        setTimeout(()=>{
                            this.isFade = false
                        }, 1500);
                    }
                })
            },
            bg8Click(){
                this.special();
                 new Ajax().sendJSON(env+'/mldjRecordScore', {
                     user_id:DataManager.user_id,
                     bid:DataManager.bid,
                     group_openid:DataManager.group_openid,
                     type:13,
                     ad_id:DataManager.ad_id,
                     sign:MD5.makeSign({
                         user_id:DataManager.user_id,
                         bid:DataManager.bid,
                         group_openid:DataManager.group_openid,
                         type:13,
                         ad_id:DataManager.ad_id
                     })
                 }, (res)=>{
                     if(res['st'] == 1){
                         this.initData();
                         this.isFade = true;
                         this.words = '集气成功';
                         setTimeout(()=>{
                             this.isFade = false
                         }, 1500);
                         this.special();
                     }
                     if(res['st'] == 2){
                         this.isFade = true;
                         this.words = '今天已参加集气';
                         setTimeout(()=>{
                             this.isFade = false
                         }, 1500);
                     }
                     window.location.href = 'http://kofd.qq.com/zlkdatasys/mct/proj_1/download.shtml?media=10028314&back=cururl'
                 })
            },
            playVideo(){

                let myVideo = document.getElementById('myVideo');
                myVideo.play();
                myVideo.addEventListener('play',()=>{
                    this.showVideo = true;
                    this.isShowAlert = true;
                });
                myVideo.addEventListener('pause',()=>{
                    this.showVideo = false;
                });
                myVideo.addEventListener('ended',()=>{
                    this.showVideo = false;
                });



              new Ajax().sendJSON(env+'/auBehavior', {
                user_id:DataManager.user_id,
                bid:DataManager.bid,
                group_openid:DataManager.group_openid,
                type:11,
                ad_id:DataManager.ad_id,
                sign:MD5.makeSign({
                  user_id:DataManager.user_id,
                  bid:DataManager.bid,
                  group_openid:DataManager.group_openid,
                  type:11,
                  ad_id:DataManager.ad_id
                })
              }, (res)=>{
                if(res['st'] == 1){
                  this.initData();
                  this.isFade = true;
                  this.words = '集气成功'
                  setTimeout(()=>{
                    this.isFade = false
                  }, 1500);
                  this.special();
                }
                if(res['st'] == 2){
                  this.isFade = true;
                  this.words = '今天已参加集气'
                  setTimeout(()=>{
                    this.isFade = false
                  }, 1500);
                }
              })

            },

            closeAlert(){
                this.showAlert = false;
            }
        },
        components:{
            alert
        }
    }
</script>
<style scoped>
    .home{
        position: relative;
        width: 100%;
        height: 100%;
        background-color: rgb(13,12,52);
        overflow-x: hidden;
        max-width: 7.5rem;
        -webkit-overflow-scrolling: touch;
        margin: 0 auto;
        text-align: center;
    }
    .homeBg{
        max-width: 7.5rem;
        background: url('../../static/bg0.jpg')no-repeat;
        background-size: 100% 100%;
    }
    .home>section{
        width: 100%;
        height: 12.05rem;
    }
    .qhContent{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        z-index: 1;
    }
    .videoContent{
        width: 100%;
        position: absolute;
        top: 3.8rem;
        z-index: 4;
        left: -.6rem;
        font-size: 0;
        line-height: 0;
        height: 3.02rem;
    }
    .bg9Img{
        position: absolute;
        left: 0.3rem;
        top: 0.8rem;
        z-index: 3;
        width: 5.05rem;
    }
    .bg4{
        width: 5.2rem;
        height: 2.86rem;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        z-index: 12;
    }

    .bg3{
        width: 5.32rem;
        height: 3.02rem;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        z-index: 3;
    }
    .bg3>video,.bg3>img{
        width: 5.32rem;
        height: 3.02rem;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        z-index: 10;
    }
    .bg2{
        width: 7.05rem;
        height: 0.51rem;
        position: absolute;
        left: 1.1rem;
        bottom: -0.51rem;
        z-index: 4;
    }
    .bg5Content{
        width:6.58rem;
        height:1.38rem;
        left: 50%;
        top:7.5rem;
        position: absolute;
        transform: translateX(-50%);
        z-index: 5;
    }
    .bg5{
        width:6.58rem;
    }
    .bg5Word{
        font-size: 0.3rem;
        text-align: left;
        color: white;
        position: absolute;
        left: 1.8rem;
        bottom: 0rem;
    }
    .bg6{
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        z-index: 5;
        width: 3.88rem;
        top: 9.5rem;
    }
    .bg7{
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        z-index: 5;
        width: 4.68rem;
        height: 1.04rem;
        top: 10.6rem;
        padding-bottom: 2rem;
    }
    .bg8{
      position: fixed;
      left: 0;
      /*z-index: 5;*/
      width: 7.5rem;
      height: 1.13rem;
      bottom: 0;
      z-index: 100;
    }
    .bg81{
        position: absolute;
        left: 0;
        z-index: 5;
        width: 7.5rem;
        height: 1.13rem;
        top: 14.5rem
    }
    .fadeDiv{
        position: fixed;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        z-index: 1100;
        background-color: white;
        border-radius: 0.2rem;
        font-size: 0.4rem;
        display: flex;
        justify-content: center;
        align-content: center;
        padding: 0.5rem 0.2rem;
    }
</style>
