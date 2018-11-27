<template>
  <div>
    <Button type="primary">primary</Button>
    <Button type="info">info</Button>
    <Button type="success">success</Button>
    <Button type="warning">warning</Button>
    <Button type="primary" disabled>primary</Button>
  </div>
</template>

<script>
import Button from '../components/Button';

export default {
  name: 'Login',
  components: {
    Button,
  },
};
</script>

<style lang="less" scoped>
</style>