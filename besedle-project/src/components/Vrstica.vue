<script setup>
import Box from "./Box.vue";
import { ref, watch } from "vue";

  const props = defineProps({
    value: String,
    solution: String,
    submmited: Boolean
  });

  const colors = ref(["","","","","",""]);

  watch(
    () => props.submmited,
    async (submitted, prevSubmmited) => {
      if(props.submmited){
        let s = props.solution;
        let v = props.value;

        let temp = ["gray", "gray","gray","gray","gray"];
        let letterPool = [];

        for (let i=0; i<5; i++){
          if (s.charAt(i) == v.charAt(i)){
            temp[i] = "green";
          } else {
            letterPool.push(s.charAt(i));
          }
        }
        for (let i=0; i<5; i++){
          if(temp[i] == "gray"){
            if(letterPool.indexOf(v.charAt(i)) != -1) {
              letterPool.splice(letterPool.indexOf(v.charAt(i)), 1);
              temp[i] = "yellow";
            }
          }

        colors.value[i] = temp[i];
        await new Promise((resolve) => setTimeout(resolve, 500));
        }


      }
    }
  );
  </script>

<template>
  <div class="wrapper">
    <box class="row" v-for="i in 5" :key="i" :letter="value[i-1]" :color=colors[i-1] />
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 580px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
}

.row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  justify-content: center;
}
</style>
