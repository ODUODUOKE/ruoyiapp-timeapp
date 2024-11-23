<template>
    <view class="flip-container">
        <view class="flip-items" v-for="(unit,unitIndex) of timeArr" :key="unitIndex" :class="{subAM:unit.isHour&&unit.isAM,subPM:unit.isHour&&!unit.isAM}">
        	<view class="item" v-for="(item,index) of unit.max + 1" :key="index" :class="{current: unit.current == index, past: unit.current - 1 == index || index==unit.max&&unit.current==0,}">
                <view class="up">
                    <view class="inner">{{index |to2digits(unit.isHour)}}</view>
                    <view class="shadow"></view>
                </view>
                <view class="down">
                    <view class="inner">{{index |to2digits(unit.isHour)}}</view>
                    <view class="shadow"></view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
  function getTimeStr(){
        let time = new Date();
        let hour = ('00' + time.getHours()).slice(-2)
        let minute = ('00' + time.getMinutes()).slice(-2)
        let second = ('00' + time.getSeconds()).slice(-2)
        return  hour + ':' + minute;
    }
	export default {
        data() {
          return {
                    timeStr:getTimeStr(),
                    timeRunner:'',
          }
        },
        filters:{
            to2digits(num,isHour){
                return isHour?num:('00'+num).slice(-2)
            }
        },
        computed:{
            timeArr(){
                return this.timeStr.split(':').map((unit,index)=>{
                    unit = Number(unit);
                    let max,current,isHour,isAM;
                    if(index===0){  // 时
                        max = 12
                        isAM = unit>12?false:true
                        current = isAM?unit:unit-12
                        isHour = true
                    }else{  //分
                        max = 59
                        current = unit
                        isHour = false
                    }
                    return {
                        max,
                        current,
                        isHour,
                        isAM,
                    }
                })
            }
        },
        onLoad() {
          // 隐藏虚拟按键
          // plus.navigator.hideSystemNavigation()
          // // 强制横屏
          // plus.screen.lockOrientation('landscape-primary');
          // // 设置屏幕常亮
          // // Android平台一定要给android.permission.WAKE_LOCK权限，并且重新打包后才会生效。
          // plus.device.setWakelock(true);
        },
        methods:{
            setTimeRunner(){
                this.timeRunner = setInterval(()=>{
                    this.timeStr = getTimeStr()
                },1000)
            }
        },
        created() {
            this.setTimeRunner()
        },
        beforeDestroy() {
            clearInterval(this.timeRunner)
        },
	}
</script>

<style>
    html,body,page{
        /* background-color: #110F11; */
        font: normal "Helvetica Neue", Helvetica, sans-serif;
        background-color: #000;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
<style lang="stylus">
    $width = 78vh;
    // $backgroundColor = #2C292C
    $backgroundColor = #0a0a0a
    // $color = #BAB7BA
    $color = #afafaf
    $time = 1s
    $height = $width;
    $fontSize = $width * 0.75;
    $lineWidth = ($width / 60);
    $radius = ($width / 13);
    $perspective = $width * 2.5;
    .flip-container
        display flex
        .flip-items
            position relative
            width $width
            height $height
            font-size $fontSize
            letter-spacing -10rpx
            font-weight bold
            border-radius $radius
            box-shadow: 0 2rpx 18rpx rgba(0, 0, 0, 0.7)
            &:first-child
                margin-right $width *0.1
            &.subPM .item .down::after
                content 'PM'
                position absolute
                bottom 12%
                left 8.5%
                font-size $fontSize *0.15
                letter-spacing normal
                color $color
            &.subAM .item .up::after
                content 'AM'
                position absolute
                top 12%
                left 8.5%
                font-size $fontSize *0.15
                letter-spacing normal
                color $color
            .item
                z-index 1
                position absolute
                top 0
                left 0
                right 0
                bottom 0
                perspective $perspective
                &:before
                    content: ''
                    position absolute
                    top (($height - $lineWidth) / 2)
                    left 0
                    z-index 9
                    width: 100%
                    height: $lineWidth
                    background-color black
                    // background-color $backgroundColor
                .up,.down
                    position absolute;
                    left 0;
                    right 0
                    height 50%;
                    overflow hidden
                .up
                    transform-origin 50% 100%
                    top 0
                .down
                    transform-origin 50% 0%
                    bottom 0
                .inner
                    position: absolute;
                    left: 0;
                    width: 100%;
                    height: $height
                    line-height $height
                    color: $color;
                    text-shadow: 0 2rpx 4rpx #000
                    text-align: center;
                    background-color: $backgroundColor
                    border-radius: $radius
                .up .inner
                    top 0
                    border-radius: 27px
                    border: 0px solid red
                    background: #2b2b2b
                .down .inner
                    bottom 0
                    border-radius: 27px
                    border: 0px solid red
                    background: #2b2b2b
                .up .shadow
                    border-top-left-radius $radius
                    border-top-right-radius $radius
                .down .shadow
                    border-bottom-left-radius $radius
                    border-bottom-right-radius $radius
        .flip-items .item
            &.past {
              z-index: 3;
            }
            &.current {
              //反转到中间时候当前秒层级最大
              animation: highter-level ($time/2) ($time/2) linear forwards;
              z-index: 2;
            }
            &.past .up {
              animation: flip-past-up ($time/2) linear both;
            }
            &.current .down {
              animation: flip-current-down ($time/2) ($time/2) linear both;
            }
            @keyframes flip-current-down {
              from{
                transform: rotateX(90deg);
              }
              to {
                transform: rotateX(0deg);
              }
            }
            @keyframes flip-past-up {
              from{
                transform: rotateX(0deg);
              }
              to {
                transform: rotateX(-90deg);
              }
            }
            @keyframes highter-level {
              from{
                z-index: 4;
              }
              to {
                z-index: 4;
              }
            }
        // 控制阴影
        .flip-items .item
            .shadow {
              position: absolute;
              width: 100%;
              height: 100%;
            }
            &.past .up .shadow {
              background: linear-gradient(rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 1) 100%);
              animation: show ($time/2) linear both;
            }
            &.past .down .shadow {
              background: linear-gradient(rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.1) 100%);
              animation: show ($time/2) linear both;
            }
            &.current .up .shadow {
              background: linear-gradient(rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 1) 100%);
              animation: hide ($time/2) 0.3s linear both;
            }
            &.current .down .shadow {
              background: linear-gradient(rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.1) 100%);
              animation: hide ($time/2) 0.3s linear both;
            }

        @keyframes show {
          from{
            opacity: 0;
          }
          to {
            opacity: 1;
          }
        }
        @keyframes hide {
          from{
            opacity: 1;
          }
          to {
            opacity: 0;
          }
        }
</style>
