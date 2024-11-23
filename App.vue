<script>
  export default {
    onLaunch: function() {
      this.initApp();
    },
    methods: {
      // 初始化应用
      initApp() {
        //隐藏虚拟按键
        plus.navigator.hideSystemNavigation()
        // 强制横屏
        plus.screen.lockOrientation('landscape-primary');
        // 设置屏幕常亮
        // Android平台一定要给android.permission.WAKE_LOCK权限，并且重新打包后才会生效。
        plus.device.setWakelock(true);
      }
    }
  }
</script>

<style lang="scss">
  @import '@/static/scss/index.scss';

  /*@font-face {
    font-family: Arvo-Bold;
    src: url('~@/static/font/Poppins-Medium.ttf');
  }*/
</style>
