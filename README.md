# nx-json-editor

## Usage example
```vue
<template>
  <div>
    <NxJsonForm
      :schema="schema"
      v-model="form"
      :disable-collapse="disableCollapse"
      :disable-edit-json="disableEditJson"
      :disable-properties="disableProperties"
    />
  </div>
</template>

<script>
import NxJsonForm from "@/components/nx-json-form/NxJsonForm";
import "bootstrap/dist/css/bootstrap.min.css";

export default {
  components: {
    NxJsonForm,
  },
  data() {
    return {
      schema: {},
      form: {},
      disableCollapse: true,
      disableEditJson: true,
      disableProperties: true,
    };
  },
  created() {
    this.setSchema();
    this.setSchemaValue();
  },
  methods: {
    setSchemaValue() {
      this.form = { name: "SchemaA", age: 20, favorite_color: "#ffa500" };
    },
    setSchema() {
      this.schema = {
        type: "object",
        required: ["name", "age", "favorite_color"],
        properties: {
          name: {
            type: "string",
            description: "First and Last name",
            minLength: 4,
            default: "Jeremy Dorn",
          },
          age: {
            type: "integer",
            default: 25,
            minimum: 18,
            maximum: 99,
          },
          favorite_color: {
            type: "string",
            format: "color",
            title: "favorite color",
            default: "#ffa500",
          },
        },
      };
    },
  },
};
</script>
```
