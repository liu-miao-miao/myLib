<template>
  <!-- <div
    class="btn"
    :class="{disable: disable}"
    @click="onClick">{{title}}</div> -->
  <button
    :class="btnClasses"
    :style="[{width: btnWidth},{height: btnHight}]"
    @click="handleClick"
    :disabled="buttonDisabled || loading"
    :autofocus="autofocus"
    :type="nativeType">
    <i v-if="loading" class="el-icon-loading"></i>
    <i v-if="icon && !loading" :class="icon"></i>
    <span v-if="$slots.default"><slot></slot></span>
  </button>
</template>
<script>
const prefixCls = 'btn';
export default {
  props: {
    type: {
      type: String,
      default: 'primary',// primary info success warning error
    },
    size: {
      type: String,
      default: 'default',// default big small mini
    },
    width: {
      type: String,
      default: '100px',
    },
    height: {
      type: String,
      default: '40px',
    },
    disabled: Boolean,
    loading: Boolean,
    autofocus: Boolean,
    round: Boolean, 
    circle: Boolean, 
  },
  computed: {
    btnClasses() {
      return [
        `${prefixCls}`,
        `${prefixCls}-${this.type}`,
        `${prefixCls}-${this.size}`,
        {
          'is-disabled': this.disabled,
          'is-loading': this.loading,
          'is-round': this.round,
          'is-circle': this.circle,
        }
      ];
    },
    btnWidth() {
      return [
        `${this.width}`,
      ];
    },
    btnHight() {
      return [
        `${this.height}`,
      ];
    },
    buttonDisabled() {
      return this.disabled;
    }
  },
  methods: {
    handleClick(event) {
      this.$emit('onClick', event);
    },
  },
};
</script>
<style lang="less" scoped>
.btn {
  background-color: #409eff;
  font-size: 20px;
  color: #fff;
  text-align: center;
  border: none;
  outline: none;
  & + & {
    margin-left: 10px;
  }
  &-info {
    background-color: #FFCC66;
  }
  &-success {
    background-color: #67c23a;
  }
  &-warning {
    background-color: #FF9966;
  }
  &-error {
    background-color: #FF9966;
  }
  &:hover {
    opacity: 0.9;
    cursor: pointer;
  }
  &.is-disabled, &.is-loading {
    opacity: .5;
    cursor: not-allowed;
  }
}
</style>