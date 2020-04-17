<template>
  <div>{{ count }} {{ object.foo }}</div>
</template>

<script>
import { ref, reactive, isRef, toRefs } from "vue";

export default {
  setup() {
    /**
     * 注意，当在模板中访问时
     * 从安装程序返回的refs会自动打开
     * 因此不需要在模板中使用.value
     * */
    const count = ref(0);
    const bar = ref('1');
    const name = ref([1,2,3]);
    const object = reactive({ foo: "bar" });

    // Convert a reactive object to a plain object
    console.log(toRefs(object));

    console.log(count, bar, name, object, object.foo);

    console.log(isRef(count));
    // expose to template
    return {
      count,
      object,
    };
  },
};
</script>
