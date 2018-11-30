<template>
  <div>
    <div>
      <h2>Button</h2>
      <p>默认</p>
      <Button type="primary">primary</Button>
      <Button type="info">info</Button>
      <Button type="success">success</Button>
      <Button type="warning">warning</Button>
      <Button type="primary" disabled>primary</Button>
      <Button type="primary" loading></Button>
    </div>
    <div>
      <p>圆角</p>
      <Button type="primary" round>primary</Button>
      <Button type="info" round>info</Button>
      <Button type="success" round>success</Button>
      <Button type="warning" round>warning</Button>
      <Button type="primary" round disabled>primary</Button>
      <Button type="primary" round loading></Button>
    </div>
    <div>
      <p>大小</p>
      <Button type="primary" round size="default">default</Button>
      <Button type="primary" round size="big" width="120px">big</Button>
      <Button type="primary" round size="small" width="80px">small</Button>
      <Button type="primary" round size="mini" width="60px">mini</Button>
    </div>
    <div>
      <p>圆形</p>
      <Button type="primary" circle>primary</Button>
      <Button type="info" circle>info</Button>
      <Button type="success" circle>success</Button>
      <Button type="warning" circle>warning</Button>
      <Button type="primary" circle disabled>primary</Button>
      <Button type="primary" circle loading></Button>
      <Button type="primary" circle width="40px"></Button>
    </div>
    <div>
      <p>禁用</p>
      <Button type="primary" round disabled>primary</Button>
      <Button type="info" round disabled>info</Button>
      <Button type="success" round disabled>success</Button>
      <Button type="warning" round disabled>warning</Button>
      <Button type="primary" round disabled>primary</Button>
      <Button type="primary" round disabled loading></Button>
    </div>
    <div>
      <p>可点击时响应点击，禁用状态下不响应</p>
      <Button type="primary" round @onClick="handleBtnClick">primary</Button>
      <Button type="primary" round @onClick="handleBtnClick" disabled>primary</Button>
    </div>
  </div>
</template>

<script>
import Button from '../components/Button';

export default {
  name: 'Login',
  components: {
    Button,
  },
  methods: {
    handleBtnClick() {
      alert('you clicked the button');
    },
  },
};
</script>

<style lang="less" scoped>
</style>