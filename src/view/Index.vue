<template>
    <div class="container">
        <img class="flower" src="../assets/huawen.png" alt="">
        <div id="imgContainer" class="img-container">
          
            <div id="imgCon" class="img-con">
                <img id="showbg" v-show="showbg" class="bg" src="../assets/changjing.png" alt="">
                <!-- <img class="people" width="70%" src="../assets/people.png" alt=""> -->
                <img @click="toBegin" class="start-bg" width="30%" src="../assets/start.gif" alt="">
            </div>
            
        </div>
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
    name: 'Index',
    data(){
        return{
            showbg: false,
            per: 0.532635987885885
        }
    },
    created(){
        //alert(location.href)
        //alert(this.$route.query.openid)
        request.post('/CefuqiAPi/SaveOpenid' ,{openid: this.$route.query.openid}).then(() => {
            //测试次数
            request.post('/CefuqiAPi/getawarddes').then(res => {
                //alert(JSON.stringify(res))
                if(res.res){
                    //this.$router.push('/begin');
                }else{
                    // alert('抽奖次数已用完')
                    this.$router.replace('/result')
                }
            }).catch()
        }).catch(err => {alert(JSON.stringify(err))})

        //
        
    },
    mounted(){
        let imgContainer = document.getElementById('imgContainer')

        let conwidth = window.getComputedStyle(imgContainer).width.split('px')[0]
        let conheight = window.getComputedStyle(imgContainer).height.split('px')[0]
        //let per = conwidth/conheight
        if((conwidth/conheight) > this.per){
            document.getElementById('showbg').style.height = conheight + 'px'
            this.showbg = true
        }else{
            document.getElementById('showbg').style.width = conwidth + 'px'
            this.showbg = true
        }
    },
    methods:{
        toBegin(){
            //this.$router.push('/begin');
            request.post('/CefuqiAPi/getawarddes').then(res => {
                //alert(JSON.stringify(res))
                if(res.res){
                    this.$router.push('/begin');
                }else{
                    alert('测试次数已用完')
                }
            }).catch(err => {alert('测试次数已用完!')})
        }
    }
}
</script>
<style lang="scss" scoped>
    @import '../scss/app.scss';
    .container{
        position: absolute;
        width: 100%;
        top: 0;
        bottom: 0;
        left: 0;
        .img-container{
            width: 90%;
            height: 95%;
            position: absolute;
            z-index: 2;
            @include centerAll;
            .img-con{
                position: absolute;
                // width: 100%;
                // height: 100%;
                @include centerAll;
                .people{
                    position: absolute;
                    bottom: 13%;
                    left: 50%;
                    transform: translateX(-50%);
                }
            }
            .bg{
                display: block;
                margin: 0;
                // position: absolute;
                // @include centerAll;
            }
            .start-bg{
                position: absolute;
                left: 50%;
                bottom: 12%;
                transform: translateX(-50%);
                z-index: 9;
            }
        }
    }
</style>
