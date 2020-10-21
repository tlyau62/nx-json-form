<template>
  <NxJsonFormInternal
    :key="id"
    @refresh="id++"
    v-bind="$attrs"
    v-on="$listeners"
  />
</template>

<script>
import NxJsonFormInternal from "./NxJsonFormInternal";

export default {
  name: "NxJsonForm",
  components: {
    NxJsonFormInternal,
  },
  data() {
    return {
      id: 0,
    };
  },
};
</script>

<style>
</style>