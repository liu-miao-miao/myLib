<template>
  <button
    :class="btnClasses"
    :style="[{width: btnWidth},{height: btnHight}]"
    @click="handleClick"
    :disabled="buttonDisabled || loading"
    :autofocus="autofocus"
    :type="nativeType">
    <i v-if="loading" class="icon-loading">
      <BtnLoading></BtnLoading>
    </i>
    <i v-if="icon && !loading" :class="icon"></i>
    <slot></slot>
  </button>
</template>

<script>
const prefixCls = 'btn';
import BtnLoading from './BtnLoading.vue';

export default {
  name: 'Button',
  props: {
    type: {
      type: String,
      default: 'primary',// primary info success warning error
    },
    width: {
      type: String,
      default: '100px',
    },
    height: {
      type: String,
      default: '40px',
    },
    size: {
      type: String,
      default: 'default',// default big small mini
    },
    nativeType: {
      type: String,
      default: 'button'
    },
    icon: {
      type: String,
      default: '',
    },
    disabled: Boolean,
    loading: Boolean,
    autofocus: Boolean,
    round: Boolean, 
    circle: Boolean,
  },
  components: {
    BtnLoading,
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
  padding: 5px 10px;
  font-size: 14px;
  color: #fff;
  text-align: center;
  border: none;
  outline: none;
  background-color: #409eff;
  vertical-align: middle;
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
    opacity: .9;
    cursor: pointer;
  }
  &.is-disabled, &.is-loading {
    opacity: .7;
    cursor: not-allowed;
  }
  &.is-round {
    border-radius: 4px;
  }
  &-big {
    padding: 8px 12px;
  }
  &.is-circle {
    border-radius: 20px;
  }
}
</style>