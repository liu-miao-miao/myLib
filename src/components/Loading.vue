<template>
  <div v-show="loading" :class="classes">
    <div :class="circularCls">
      <svg viewBox="25 25 50 50">
        <circle cx="50" cy="50" r="20" fill="none" class="circle"></circle>
      </svg>
      <p v-if="text" :class="textCls">{{text}}</p>
    </div>
  </div>
</template>

<script>
const prefixCls = 'loading';

export default {
  name: 'Loading',
  props: {
    // loading: {
    //   type: Boolean,
    //   default: false,
    // },
    // text: {
    //   type: String,
    //   default: 'Loading',
    // },
    // minHeight: {
    //   type: Number,
    //   default: '200',
    // },
  },
  data() {
    return {
      loading: false,
      text: 'Loading',
    };
  },
  mounted() {
    this.$bus.$on('showLoading', ({ text }) => {
      this.showLoading(text);
    });
    this.$bus.$on('LoadingClose', () => {
      this.hideLoading();
    });
  },
  beforeDestroy() {
    this.$bus.$off('showLoading');
  },
  methods: {
    showLoading(text) {
      this.loading = true;
      this.text = text;
    },
    hideLoading() {
      this.loading = false;
    },
  },
  computed: {
    classes() {
      return [
        `${prefixCls}`,
      ];
    },
    circularCls() {
      return [
        `${prefixCls}-circular`,
      ];
    },
    textCls() {
      return [
        `${prefixCls}-text`,
      ];
    },
  },
};
</script>

<style lang="less" scoped>
@loading-prefix: ~"loading";
.@{loading-prefix} {
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  top: 0;
  z-index: 9999;
  text-align: center;
  background-color: hsla(0,0%,100%,.6);
  .@{loading-prefix}-circular {
    position: absolute;
    top: 50%;
    left: 50%;
    height: 42px;
    transform: translate(-50%, -50%);
    > svg {
      width: 42px;
      height: 42px;
      transition: opacity 0.5s;
      transform-origin: center center;
      opacity: 1;
      animation: loading-rotate 2s linear infinite;
      .circle {
        stroke-dasharray: 90, 150;
        stroke-dashoffset: 0;
        stroke-width: 2;
        stroke: #4285F4;
        stroke-linecap: round;
        animation: loading-path-rotate 1.5s ease-in-out infinite;
      }
    }
  }
}
@keyframes loading-rotate {
  to {
    transform: rotate(1turn);
  }
}
@keyframes loading-path-rotate {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -35px;
  }
  to {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -124px;
  }
}
</style>