<template>
  <div :class="classes">
    <div :class="containerCls" :style="{height: `${height}px`}">
      <div :class="listCls" @mouseover="stopAutoplay" @mouseout="startAutoplay" :key="effect">
        <div
          :class="listItemCls"
          v-for="(item,index) in carouselList"
          :key="index"
          @click="clickTrigger(index, item)"
        >
          <a :href="item.linkUrl" target="_blank">
            <div
              v-if="!$scopedSlots.item"
              class="carousel-bg"
              :style="{backgroundImage:`url(${item.url})`}"
            ></div>
            <slot :carousel="item" name="item" :index="index"></slot>
          </a>
        </div>
      </div>
      <div
        class="carousel-arrow"
        :class="arrowCls">
        <div class="h-icon-left" @click="prev"></div>
        <div class="h-icon-right" @click="next"></div>
      </div>
    </div>
    <ul class="carousel-pagination" :class="paginationCls">
      <li
        class="carousel-pagination-item"
        v-for="(item, index) of datas"
        :key="index"
        :class="{'active': isActive(index)}"
        @mouseover="triggerChange('hover', index+1)"
        @click="triggerChange('click', index+1)"
      >
        <slot v-if="$scopedSlots.page" :carousel="item" name="page"></slot>
        <span v-else></span>
      </li>
    </ul>
  </div>
</template>
<script>
const prefixCls = "carousel";

/* eslint-disable */
export default {
  props: {
    height: {   // 轮播图容器高度
      type: Number,
      default: 300,
    },
    speed: {  // 自动播放切换的时间
      type: Number,
      default: 3000,
    },
    autoplay: {  // 是否自动播放
      type: Boolean,
      default: true,
    },
    changeSpeed: {  // 图片切换速度
      type: Number,
      default: 500,
    },
    arrow: {  // 左右切换按钮展示激活方式
      type: String,
      default: "hover", // hidden (隐藏), always (一直显示), hover (悬停)
    },
    pageTheme: {  // 底部指示器样式
      type: String,
      default: "square", // circle, hidden, square, custom(自定义)
    },
    datas: { // 图片数据
      type: Array,
    },
    isHoverStop: {  // 鼠标悬停时是否停止滚动
      type: Boolean,
      default: true,
    },
    paginationTrigger: {  // 底部指示器的触发方式
      type: String,
      default: "click", // hover, click
    },
    effect: {  // 切换效果
      type: String,
      default: "scroll", // scroll, fade
    }
  },
  data() {
    return {
      activeIndex: 1,
      scrollTimeout: null,
      redirectTimeout1: null,
      redirectTimeout2: null,
    };
  },
  computed: {
    classes() {
      return [`${prefixCls}`];
    },
    containerCls() {
      return [`${prefixCls}-container`];
    },
    listCls() {
      return [`${prefixCls}-list`, `${prefixCls}-scroll-list`];
    },
    listItemCls() {
      return [`${prefixCls}-item`];
    },
    paginationCls() {
      return `carousel-pagination-${this.pageTheme}`;
    },
    arrowCls() {
      return `carousel-arrow-${this.arrow}`;
    },
    carouselList() {
      if (this.datas.length === 0) {
        return [];
      }
      let datas = this.datas;
      return [datas[this.datas.length - 1], ...datas, datas[0]];
    },
    carouselItem() {
      if (this.activeIndex === this.carouselList.length - 1) {
        this.activeIndex = 1;
      } else if (this.activeIndex === 0) {
        this.activeIndex = this.carouselList.length - 2;
      }
      return this.datas[this.activeIndex - 1];
    },
  },
  watch: {
    autoplay() {
      if (this.autoplay) {
        this.startAutoplay(true);
      } else {
        this.stopAutoplay(true);
      }
    },
    effect() {
      clearTimeout(this.scrollTimeout);
      clearTimeout(this.redirectTimeout1);
      clearTimeout(this.redirectTimeout2);
      this.init();
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.init();
    });
  },
  beforeDestroy() {
    clearTimeout(this.scrollTimeout);
    clearTimeout(this.redirectTimeout1);
    clearTimeout(this.redirectTimeout2);
    window.removeEventListener("resize", this.resizeEvent);
  },
  methods: {
    clickTrigger(index, data) {
      this.$emit("click", index, data);
    },
    isActive(index) {
      let datas = this.datas;
      let activeIndex = this.activeIndex;
      return (
        index + 1 === activeIndex ||
        (activeIndex === 0 && index === datas.length - 1) ||
        (activeIndex === datas.length + 1 && index === 0)
      );
    },
    init() {
      this.startAutoplay(true);
      setTimeout(() => {
        this.change({ index: this.activeIndex, immediately: true });
      }, 300);
      window.addEventListener("resize", this.resizeEvent, false);
    },
    stopAutoplay(force = false) {
      if (this.isHoverStop || force) {
        clearTimeout(this.scrollTimeout);
      }
    },
    startAutoplay(force = false) {
      if ((this.isHoverStop || force) && this.autoplay) {
        clearTimeout(this.scrollTimeout);
        this.scrollTimeout = setTimeout(() => {
          this.next();
        }, this.speed);
      }
    },
    resizeEvent() {
      this.change({ index: this.activeIndex, immediately: true });
    },
    scroll(index, immediately) {
      this.activeIndex = index;
      const itemWidth = this.$el.clientWidth;
      const width = index * itemWidth;
      switch (this.effect) {
        case "scroll": {
          const listDom = this.$el.querySelector(".carousel-scroll-list");
          if (immediately) {
            listDom.style.transitionDuration = "0ms";
          } else {
            listDom.style.transitionDuration = `${this.changeSpeed}ms`;
          }
          listDom.style.transform = `translate3d(${-width}px, 0px, 0px)`;
          break;
        }
        case "fade": {
          const listDom = this.$el.querySelector(".carousel-scroll-list");
          if (immediately) {
            listDom.style.transitionDuration = "0ms";
          } else {
            listDom.style.transitionDuration = `${this.changeSpeed}ms`;
          }
          listDom.style.transform = `translate3d(${-width}px, 0px, 0px)`;
          break;
        }
        default: {
          break;
        }
      }
    },
    change({ index = 1, immediately = false }) {
      if (this.activeIndex === this.carouselList.length - 1) {
        this.scroll(1, true);
      } else if (this.activeIndex === 0) {
        this.scroll(this.carouselList.length - 2, true);
      }
      clearTimeout(this.scrollTimeout);
      clearTimeout(this.redirectTimeout1);
      clearTimeout(this.redirectTimeout2);
      if (immediately) {
        this.scroll(index, immediately);
      } else {
        this.scroll(index, immediately);
        this.$emit("change", index, this.carouselList[this.activeIndex]);
        // 当翻页到第一页的时候，默默切换至真实的第一页
        if (this.activeIndex === this.carouselList.length - 1) {
          this.redirectTimeout1 = setTimeout(() => {
            this.scroll(1, true);
          }, this.changeSpeed + 100);
        } else if (this.activeIndex === 0) {
          this.redirectTimeout2 = setTimeout(() => {
            this.scroll(this.carouselList.length - 2, true);
          }, this.changeSpeed + 100);
        }
      }
      this.startAutoplay(true);
    },
    changePageByStep(step) {
      let activeIndex = this.activeIndex + step;
      if (activeIndex >= this.carouselList.length) {
        activeIndex = 2;
      } else if (activeIndex < 0) {
        activeIndex = this.carouselList.length - 3;
      }
      this.change({ index: activeIndex });
    },
    triggerChange(triggerType, index) {
      if (this.paginationTrigger === triggerType) {
        this.change({ index });
      }
    },
    prev() {
      this.changePageByStep(-1);
    },
    next() {
      this.changePageByStep(1);
    },
  }
};
</script>

<style lang="less" scoped>
@carousel-prefix: ~"carousel";
.@{carousel-prefix} {
  position: relative;
  & &-container {
    position: relative;
    width: 100%;
    height: 100%;
    z-index: 1;
    overflow: hidden;
  }
  & &-list {
    display: flex;
    position: relative;
    width: 100%;
    height: 100%;
    z-index: 1;
    transition-property: transform;
    box-sizing: content-box;
    .@{carousel-prefix}-item {
      // background-color: @gray-color;
      position: relative;
      width: 100%;
      height: 100%;
      flex-shrink: 0;
      background-position: center;
      background-repeat: no-repeat;
      .@{carousel-prefix}-bg {
        height: 100%;
        background-position: center;
        background-size: cover;
      }
      .@{carousel-prefix}-bg-pointer {
        cursor: pointer;
      }
      &.@{carousel-prefix}-effect-item {
        position: absolute;
      }
    }
  }
  .@{carousel-prefix}-arrow {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transition: 0.3s;
    .h-icon-left,
    .h-icon-right {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      width: 32px;
      height: 32px;
      z-index: 2;
      color: #fff;
      font-size: 32px;
      background: #eee;
      cursor: pointer;
      opacity: 0.4;
      transition: 0.5s;
      &:hover {
        opacity: 1;
      }
    }
    .h-icon-left {
      left: 2%;
    }
    .h-icon-right {
      right: 2%;
    }
    &.@{carousel-prefix}-arrow-hover {
      opacity: 0;
    }
  }
  &:hover &-arrow-hover {
    opacity: 1;
  }
  .@{carousel-prefix}-arrow-hidden {
    display: none !important;
  }
}

.@{carousel-prefix}-pagination {
  position: relative;
  z-index: 3;

  &-circle &-item,
  &-square &-item {
    display: inline-block;
    position: relative;
    height: 15px;
    margin-right: 10px;
    opacity: 0.4;
    cursor: pointer;
    transition: 0.5s;
    > span {
      display: inline-block;
      border-radius: 4px;
      background-color: #fff;
    }
    &.active {
      opacity: 1;
    }
    &:last-of-type {
      margin-right: 0;
    }
  }

  &-circle,
  &-square {
    position: absolute;
    bottom: 5%;
    left: 50%;
    transform: translateX(-50%);
    z-index: 2;
  }
  &-circle &-item > span {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }
  &-square &-item > span {
    width: 25px;
    height: 3px;
  }
  &-hidden {
    display: none;
  }
}
.@{carousel-prefix}-effect-fade-enter-active,
.@{carousel-prefix}-effect-fade-leave-active {
  transition: opacity 1s;
}
.@{carousel-prefix}-effect-fade-enter,
.@{carousel-prefix}-effect-fade-leave-to {
  opacity: 0;
}
</style>