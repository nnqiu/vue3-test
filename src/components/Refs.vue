<template>
    <div ref="input">ref节点</div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    // 创建一个 DOM 引用
    const input = ref(null);
    // 在 DOM 首次加载完毕之后，才能获取到元素的引用
    onMounted(() => {
      // input.value 是原生DOM对象
      console.log(input.value); 
    });
    return {
      input,
    };
  },

  mounted() {
    console.log(this, 'this');
  }
};

/**
 * 通过 ref() 还可以引用页面上的元素或组件
 * When using the Composition API, the concept of reactive refs and template refs are unified. 
    In order to obtain a reference to an in-template element or component instance, 
    we can declare a ref as usual and return it from setup():
*/

</script>

