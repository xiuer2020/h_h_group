<template>
  <div class="home" @mousedown="mousedownHandle">
    <div class="swiper-wrapper">
      <div
        class="swiper-slide viedo"
        :class="[
          activeIndex === 0 ? 'active' : '',
          activeIndex > 0 ? 'less' : '',
        ]"
      >
        <video
          src="../assets/mp4/原首页视频.mp4"
          controls
          autoplay
          muted
        ></video>
      </div>
      <div
        class="swiper-slide"
        :class="[
          activeIndex === 1 ? 'active' : '',
          activeIndex > 1 ? 'less' : '',
        ]"
      >
        <img
          src="../assets/img/carousel-1.png"
          alt=""
          ondrop="return false"
          οndragstart="return false;"
          οncοntextmenu="return false;"
        />
      </div>
      <div
        class="swiper-slide"
        :class="[
          activeIndex === 2 ? 'active' : '',
          activeIndex > 2 ? 'less' : '',
        ]"
      >
        <img
          src="../assets/img/home-bg3.png"
          alt=""
          ondrop="return false"
          οndragstart="return false;"
          οncοntextmenu="return false;"
        />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  data: () => ({
    startY: 0,
    // 触摸开始y轴坐标
    endY: 0,
    // 结束开始y轴坐标
    minDiffY: 100,
    // 最小y轴差值
    activeIndex: 0,
    // 列表图激活项下标
    scrollTop: 0,
    // 滚轮y轴坐标
  }),
  watch: {},
  computed: {
    diffY: function () {
      return this.endY - this.startY;
    },
    // y轴坐标差值
  },
  methods: {
    mousedownHandle: function (e) {
      this.startY = e.pageY;
      // 设置触摸开始y轴坐标

      this.$root.$el.onmouseup = (ev) => {
        this.endY = ev.pageY;
        // 设置触摸开始y轴坐标
        // console.log(this.diffY);

        if (this.diffY >= this.minDiffY) {
          console.log("差值大于100");
          this.activeIndex < 1 ? 0 : this.activeIndex--;
        } else if (this.diffY <= -this.minDiffY) {
          console.log("差值大于100");
          this.activeIndex > 2 ? 3 : this.activeIndex++;
        }
      };
    },

    wheel: function (e) {
      document.getElementsByClassName("home")[0].scrollTop = 0;

      if (e.wheelDeltaY > 0) {
        this.activeIndex < 1 ? 0 : this.activeIndex--;
      } else {
        this.activeIndex > 2 ? 3 : this.activeIndex++;
      }
    },
    debounce: function (func, delay) {
      let timer;
      console.log(func, delay);
      return (...arg) => {
        if (timer) {
          clearTimeout(timer);
        }
        timer = setTimeout(() => {
          func.apply(this, arg);
        }, delay);
      };
    },
    // 防抖
  },
  created() {},
  mounted() {
    document.images.forEach((el) => {
      el.ondragstart = () => false;
    });
    // 禁止拖拽页面图片

    document.onmousewheel = this.debounce(this.wheel, 1000);
    // 滚轮事件
  },
};
</script>
<style scoped lang="less">
.home {
  .swiper-wrapper {
    .swiper-slide {
      position: relative;
      left: 0;
      top: 0;

      height: 100vh;
      transition: transform 1.5s cubic-bezier(1, 0.14, 0.51, 1.56),
        opacity 1.5s cubic-bezier(1, 0.14, 0.51, 1.56), top 1s linear;
      overflow: hidden;
      background: #fff;
      opacity: 0.7;

      &.less {
        transform: scale(0.5);
        opacity: 0;
        &:nth-child(1) {
          top: 0;
        }
        &:nth-child(2) {
          top: -100vh;
        }
        &:nth-child(3) {
          top: -200vh;
        }
      }

      &.active {
        transform: scale(1);
        opacity: 1;
        &:nth-child(1) {
          top: 0;
        }
        &:nth-child(2) {
          top: -100vh;
        }
        &:nth-child(3) {
          top: -200vh;
        }
      }
    }
  }
}
</style>
