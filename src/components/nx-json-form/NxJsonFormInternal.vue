<template>
  <div></div>
</template>

<script>
import { JSONEditor } from "@json-editor/json-editor";

export default {
  name: "NxJsonFormInternal",
  props: {
    schema: {
      type: Object,
    },
    value: {
      type: Object,
    },
    disableCollapse: {
      type: Boolean,
      default: true,
    },
    disableEditJson: {
      type: Boolean,
      default: true,
    },
    disableProperties: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {};
  },
  mounted() {
    if (Object.keys(this.schema).length === 0) {
      return;
    }

    this.editor = new JSONEditor(this.$el, {
      schema: this.schema,
      theme: "bootstrap4",
      disable_collapse: this.disableCollapse,
      disable_edit_json: this.disableEditJson,
      disable_properties: this.disableProperties,
    });

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
      if (this.editor) {
        this.editor.setValue(value);
      }
    },
    disableCollapse() {
      this.$emit("refresh");
    },
    disableEditJson() {
      this.$emit("refresh");
    },
    disableProperties() {
      this.$emit("refresh");
    },
  },
  beforeDestroy() {
    if (this.editor) {
      this.editor.destroy();
    }
  },
};
</script>

<style>
</style>