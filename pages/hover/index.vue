<script setup lang="ts">
const NUM = 100;
const inputsState = ref<{
  checked: boolean;
  intervalId: null | number;
}[]>([]);
onMounted(() => {
  let list = []
  for (let i = 0;  i < NUM;i++) {
    list[i] = {
      checked: false,
      intervalId: null,
    };
  }
  inputsState.value = list
});
</script>
<template>
  <div class="wrapper">
    <input type="checkbox" v-for="(item,index) in inputsState" :key="index" :checked="item.checked"/>
  </div>
</template>
<style scoped lang="scss">
.wrapper{
  
}
input{
  display: block;
  width: 14px;
  height: 14px;
}
</style>
