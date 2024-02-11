<script setup lang="ts">
const COL = 30;
const ROW = 30;

const COLOR_NUM = 7;

const LEAVING_DURATION = 900;
const COLOR_CHANGE_INTERVAL = 300;

let colorIndex = 1;

const COLOR_LIST = [
  "hsl(23deg, 97%, 55%)",
  "hsl(55deg, 97%, 55%)",
  "hsl(126deg, 97%, 55%)",
  "hsl(177deg, 97%, 55%)",
  "hsl(229deg, 97%, 55%)",
  "hsl(280deg, 97%, 55%)",
  "hsl(332deg, 97%, 55%)",
];

const color1 = ref<string>("");
const color2 = ref<string>("");
const color3 = ref<string>("");
const color4 = ref<string>("");
const color5 = ref<string>("");
const color6 = ref<string>("");
const color7 = ref<string>("");

const colorIntervalId = ref<null | number>(null);

const inputsState = ref<
  {
    checked: boolean;
    className: string;
    intervalId: null | number;
    intervalId2: null | number;
    intervalId3: null | number;
  }[]
>([]);

const enterCheckBox = (index: number) => {

  const intervalId = inputsState.value[index].intervalId;
  const intervalId2 = inputsState.value[index].intervalId2;
  const intervalId3 = inputsState.value[index].intervalId2;
  if (intervalId) {
    clearInterval(intervalId);
  }
  if (intervalId2) {
    clearInterval(intervalId2);
  }
  if (intervalId3) {
    clearInterval(intervalId3);
  }
  inputsState.value[index].checked = true;
  inputsState.value[index].className = "enter";
  requestAnimationFrame(() => {
    inputsState.value[index].className = "active";
  });
};

const leaveCheckBox = (index: number) => {
  inputsState.value[index].intervalId = window.setTimeout(() => {
    inputsState.value[index].className = "leaving";
  }, LEAVING_DURATION/3);
  inputsState.value[index].intervalId2 = window.setTimeout(() => {
    inputsState.value[index].className = "leaving";
  }, LEAVING_DURATION/3*2);
  inputsState.value[index].intervalId3 = window.setTimeout(() => {
    inputsState.value[index].checked = false;
    inputsState.value[index].className = "";
  }, LEAVING_DURATION);
};

onMounted(() => {
  let list = [];
  for (let i = 0; i < COL * ROW; i++) {
    list[i] = {
      checked: false,
      className: "",
      intervalId: null,
      intervalId2: null,
      intervalId3: null,
    };
  }
  inputsState.value = list;

  colorIntervalId.value = window.setInterval(() => {
    color1.value = COLOR_LIST[(COLOR_NUM - colorIndex) % COLOR_NUM];
    color2.value = COLOR_LIST[(COLOR_NUM - colorIndex + 1) % COLOR_NUM];
    color3.value = COLOR_LIST[(COLOR_NUM - colorIndex + 2) % COLOR_NUM];
    color4.value = COLOR_LIST[(COLOR_NUM - colorIndex + 3) % COLOR_NUM];
    color5.value = COLOR_LIST[(COLOR_NUM - colorIndex + 4) % COLOR_NUM];
    color6.value = COLOR_LIST[(COLOR_NUM - colorIndex + 5) % COLOR_NUM];
    color7.value = COLOR_LIST[(COLOR_NUM - colorIndex + 6) % COLOR_NUM];
    if (colorIndex === 7) {
      colorIndex = 1;
    } else {
      colorIndex++;
    }
  }, COLOR_CHANGE_INTERVAL);

  onUnmounted(() => {
    if (colorIntervalId.value) {
      clearInterval(colorIntervalId.value);
    }
  });
});
</script>
<template>
  <div class="wrapper">
    <div class="inputWrapper">
      <input
        type="checkbox"
        v-for="(item, index) in inputsState"
        :key="index"
        :checked="item.checked"
        :class="item.className"
        @pointerenter="
          () => {
            enterCheckBox(index);
          }
        "
        @pointerleave="
          () => {
            leaveCheckBox(index);
          }
        "
      />
    </div>
  </div>
</template>
<style scoped lang="scss">
.wrapper {
  height: 100vh;
  display: grid;
  place-items: center;
}
.inputWrapper {
  display: flex;
  flex-wrap: wrap;
  width: calc(16px * v-bind(COL));
}
input {
  display: block;
  width: 14px;
  height: 14px;
  margin: 1px;

  &.enter {
    transition: accent-color 0s linear;
    accent-color: v-bind(color1);
  }

  &.active {
    transition: accent-color 0.3s linear;
    accent-color: v-bind(color1);
    box-shadow: 0 0 10px 2px v-bind(color1);
  }
  &.leaving {
    transition: accent-color 0.3s linear;
    accent-color: v-bind(color2);
    box-shadow: 0 0 10px 2px v-bind(color2);
  }
  &.leaving2 {
    accent-color: v-bind(color3);
    box-shadow: 0 0 10px 2px v-bind(color3);
  }
}

@keyframes leaving {
  0% {
    accent-color: v-bind(color1);
  }
  14% {
    accent-color: v-bind(color2);
  }
  29% {
    accent-color: v-bind(color3);
  }
  43% {
    accent-color: v-bind(color4);
  }
  58% {
    accent-color: v-bind(color5);
  }
  72% {
    accent-color: v-bind(color6);
  }
  86% {
    accent-color: v-bind(color5);
  }
}
</style>
