<template>
  <div id="app">
    <div id="nav">
      <p @click="handleAdd">count: {{ count }}</p>
      <p>{{ bigCount }}</p>
    </div>
  </div>
</template>

<script>
  import { ref, computed } from 'vue';
  import { effect } from '@vue/reactivity';

  // 侦听值变化执行getter，应该track到对应值的依赖中，根据immediate判断
  // watch effect是否为立即执行，非立即执行需要scheduler来控制时机，
  // deep控制是否深度监听
  // 伪代码：
  // effect(callback, { lazy: !immediate })
  // proxy getter -> track
  // 调用方式
  // const res = watch(val, (newVal) => {
  //   // your code
  // }, options);
  export default {
    setup() {
      const count = ref(1); // B -> dep
      // A -> dep
      const bigCount = computed(/* B */() => {
        return count.value + 10;
      });
      const handleAdd = () => {
        count.value++;
      };
      // 此时activeEffect是A
      effect((/* A */) => {
        alert(bigCount.value);
      });
      return {
        count,
        bigCount,
        handleAdd
      };
    }
  }
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
