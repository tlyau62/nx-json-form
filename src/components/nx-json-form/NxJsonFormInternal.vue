<template>
  <div></div>
</template>

<script>
import { JSONEditor } from "@json-editor/json-editor";

export default {
  name: "NxJsonFormInternal",
  props: ["schema", "value"],
  data() {
    return {
      editor: {},
    };
  },
  mounted() {
    this.editor = new JSONEditor(this.$el, { schema: this.schema });

    this.editor.on("change", () => {
      this.$emit("input", this.editor.getValue());
    });

    this.editor.setValue(this.value);
  },
  watch: {
    schema() {
      this.$emit("refresh");
    },
    value(value) {
      this.editor.setValue(value);
    },
  },
  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>

<style>
</style>