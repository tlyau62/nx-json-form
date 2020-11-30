<template>
  <div></div>
</template>

<script>
import { JSONEditor } from "@json-editor/json-editor";
import $ from "jquery";
import _ from "lodash";

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
    disableTitle: {
      type: Boolean,
      default: true,
    },
    showOptIn: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      editor: null,
      editorOriginalValue: {},
      cachedValue: {},
    };
  },
  mounted() {
    if (!this.schema || Object.keys(this.schema).length === 0) {
      return;
    }

    this.editor = new JSONEditor(this.$el, {
      schema: this.schema,
      theme: "bootstrap4",
      disable_collapse: this.disableCollapse,
      disable_edit_json: this.disableEditJson,
      disable_properties: this.disableProperties,
      disable_title: this.disableTitle,
      show_opt_in: this.showOptIn,
    });
  },
  watch: {
    editor(editor) {
      if (this.editor) {
        this.editor.on("change", () => {
          const rawValue = this.editor.getValue();
          const value = _.cloneDeepWith(rawValue, (v) => {
            if (v === undefined) {
              return null;
            }
          });

          this.cachedValue = value;
          // this.validate(value);
          this.$emit("input", value); // field changes value
        });
        this.editorOriginalValue = this.getDefaultValue();
        this.setEditorValue(this.value);
        this.toggleTitle(this.disableTitle);
      }
    },
    schema() {
      this.$emit("refresh");
    },
    value(value) {
      if (this.editor) {
        this.setEditorValue(value); // prop changes value
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
    disableTitle(val) {
      this.toggleTitle(val);
    },
  },
  methods: {
    toggleTitle(val) {
      $(this.$el)
        .find(".je-object__title label")
        .first()
        [val ? "hide" : "show"]();
    },
    setEditorValue(value) {
      if (value === this.cachedValue) {
        return;
      }

      const { editor } = this;
      const assigned = helper.cloneAndAssign(this.editorOriginalValue, value);
      const errors = this.validate(assigned);

      if (!errors.length) {
        editor.setValue(assigned);
      }
    },
    validate(value) {
      const errors = this.editor.validate(value);

      this.$parent.$emit("validation", errors);

      return errors;
    },
    getDefaultValue(schema) {
      const valueEditor = new JSONEditor($("<div></div>")[0], {
        schema: this.schema,
        required_by_default: true,
      });
      const value = valueEditor.getValue();

      valueEditor.destroy();

      return value;
    },
  },
  beforeDestroy() {
    if (this.editor) {
      this.editor.destroy();
    }
  },
};

const helper = {
  cloneAndAssign(object, sources) {
    const deleted = {};
    const assigned = _.assignWith(
      _.cloneDeep(object),
      sources,
      (objValue, srcValue) => {
        if (objValue === undefined) {
          return deleted;
        }
      }
    );

    return _.omitBy(assigned, (val) => val === deleted);
  },
};
</script>

<style></style>
