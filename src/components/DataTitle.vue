<template>
  <div class="text-center">
    <h2 class="text-3xl font-bold">{{ text }}</h2>
    <div class="text-3xl mt-4 mb-10">
      {{ timestamp }}
    </div>
  </div>
</template>

<script>
import moment from "moment";
import { computed } from "@vue/runtime-core";

export default {
  name: "DataTitle",
  props: ["text", "dataDate"],
  setup({ dataDate }) {
    return {
      timestamp: computed(() => {
        return moment(dataDate).format("MMM Do YYYY, h:mm:ss a");
      }),
    };
  },
  // computed: {
  //     timestamp() {
  //         return moment(this.dataDate).format('MMM Do YYYY, h:mm:ss a')
  //     }
  // }
};
</script>

<style>
</style>