<template>
  <div>{{ count }} {{ object.foo }}</div>
</template>

<script>
import { ref, reactive, isRef, toRefs, watchEffect } from "vue";

export default {
  setup(props) {
    /**
     * 注意，当在模板中访问时
     * 从安装程序返回的refs会自动打开
     * 因此不需要在模板中使用.value
     * 为什么使用value，因为如果是number类型的，赋值了失去了相应式，所以把值放到.value里面，这样始终可以获得最新的计算结果
     * */

    /**
     * ref 单数据
     * 
     * reactive 多数据 相当于observable
     * 
     * watchEffect dom渲染完
    */
    const count = ref(0);
    const bar = ref('1');
    const name = ref([1,2,3]);
    const object = reactive({ foo: "bar" });

    // Convert a reactive object to a plain object
    console.log(toRefs(object));

    console.log(count, bar, name, object, object.foo);

    console.log(isRef(count));
    console.log(this, 'this是undifined');

    // 检测props的改动，监听 （类似react的useEffect）
    watchEffect(() => {
      console.log(props);
    })
    // expose to template
    // 返回的对象属性会合并到组件上下文
    return {
      count: count,
      object: object,
    };
  },

  mounted() {
    console.log(this, 'this');
  }
};
// setup
/**
 * 新组件选项，也是其他api的入口，所有操作都在setup函数内部定义和执行
 * vue3也用vue2.x代替new Vue()
 * 
 * setup第一个参数是props
 * 
 * setup在组件实例创建时调用，相当于created和beforeCreate
 * 
 * 返回一个对象，对象里的属性直接暴露上下文
*/
/**
 * this 问题
 * vue3的this就是类似function的this，指向windows
 * 
 * .value原理
function computed(getter) {
  const ref = {
    value: null,
  }
  watchEffect(() => {
    ref.value = getter()
  })
  return ref
}
*/
</script>

