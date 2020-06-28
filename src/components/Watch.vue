<template>
  <div></div>
</template>

<script>
import { reactive, watch } from "vue";

export default {
  setup() {
    const state = reactive({ count: 0 });

    // 默认lazy
    watch(
        () => state.count,
        (count, preCount) => {
            console.log(count, preCount);
        },
        { 
            // 捕获深层的
            deep: true,
            // 非lazy，初始化就执行
            immediate: true,
        }
    )

    setTimeout(() => {
        state.count++;
        // 现在就会触发回调
    }, 5000);
  },

  mounted() {
    console.log(this, 'this');
  }
};

/**
 * watch
 * v2 
 * watch: {
 *  name: {handler: function(new, old) {}, immediate: true} 初始化执行
 * }
 * 
 * v3 
*/

</script>

