<template>
  <svg viewBox="25 25 50 50">
    <circle cx="50" cy="50" r="20" fill="none" class="circle"></circle>
  </svg>
</template>

<script>
export default {
  name: 'BtnLoading',
};
</script>

<style lang="less" scoped>
svg {
  width: 20px;
  height: 20px;
  transition: opacity 0.5s;
  transform-origin: center center;
  opacity: 1;
  animation: loading-rotate 2s linear infinite;
  .circle {
    stroke-dasharray: 90, 150;
    stroke-dashoffset: 0;
    stroke-width: 2;
    stroke: #fff;
    stroke-linecap: round;
    animation: loading-path-rotate 1.5s ease-in-out infinite;
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
