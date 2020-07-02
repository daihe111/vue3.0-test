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
