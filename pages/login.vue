<template>

<view style="background: black">
  <view class="time-container">
      <p>
        <font style="color:white;font-size: 30px;font-weight: 750;">{{ ampm }}</font>
        <font style="color:white;font-size: 200px;font-weight: 750;">{{ currTime }}</font>
        <font style="color:#848484;font-size: 30px;font-weight: 750;">{{ currSecond }}</font>
      </p>
  </view>
</view>

</template>

<script>
  export default {
    data() {
      return {
        currentTime: '',
        date: '',
        currTime: '',
        currSecond: '',
        ampm: ''
      }
    },
    computed: {

    },
    created() {
      this.updateTime();
      this.timer = setInterval(this.updateTime, 1000);
    },
    beforeDestroy() {
      clearInterval(this.timer);
    },
    onLoad() {
      //隐藏虚拟按键
      plus.navigator.hideSystemNavigation()
      // 强制横屏
      plus.screen.lockOrientation('landscape-primary');
      // 设置屏幕常亮
      // Android平台一定要给android.permission.WAKE_LOCK权限，并且重新打包后才会生效。
      plus.device.setWakelock(true);

    },
    methods: {
      nowDate() {
        var getTime = new Date().getTime(); //获取到当前时间戳
        var time = new Date(getTime); //创建一个日期对象
        var year = time.getFullYear(); // 年
        var month = (time.getMonth() + 1).toString().padStart(2, '0'); // 月
        var date = time.getDate().toString().padStart(2, '0'); // 日
        // var hour = time.getHours().toString().padStart(2, '0'); // 时
        var standardHour = (time.getHours() % 12) || 12; // 转换为12小时制
        var hour = standardHour.toString(); // 不需要格式化补0，按照实际时间显示数字
        var minute = time.getMinutes().toString().padStart(2, '0'); // 分
        var second = time.getSeconds().toString().padStart(2, '0'); // 秒


        this.ampm = hour >= 12 ? 'PM' : 'AM';
        this.date = year + "-" + month + "-" + date;
        this.currTime = hour + "  " + minute;
        this.currSecond = second;

        this.currentTime =  (
            year + "-" + month + "-" + date + " " + hour + ":" + minute + ":" + second
        )
      },
      updateTime() {
        this.nowDate();
      },
    }
  }
</script>

<style lang="scss">
.time-container {
  background: #000000cf;
  display: flex;
  position: fixed;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  height: 100vh;
}

p {
  text-align: center;
  font-family: Arvo-Bold;
}



</style>
