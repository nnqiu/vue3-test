<template>
  <div></div>
</template>

<script>
import { computed, ref, readonly, watchEffect } from "vue";

export default {
  setup() {
    // 一个ref实例
    const count = ref(1);
    // 一个根据ref自动计算并返回新ref
    const plusOne = computed(() => count.value + 1);

    const num = ref(100);
    const copyNum = readonly(num);

    watchEffect(() => {
        console.log('readOnly', copyNum);
    })

    console.log(plusOne);

    // plusOne经过包装，是只读的，只能get不能set
    plusOne.value++;
  },

  mounted() {
    console.log(this, 'this');
  }
};

/**
 * 计算属性，没有什么变化，
 * v2中computed是一个对象，直接在vue中获取
 * v3抽象成api，需要引入
 * 
*/

/**
 * readonly
 * 接收ref / reactive包装对象，返回一个只读的响应式对象
*/

</script>

