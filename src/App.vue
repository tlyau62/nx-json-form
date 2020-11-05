<template>
  <div id="app" class="container">
    <div>
      <h4>Form value</h4>
      <div>
        {{ form }}
      </div>
    </div>

    <NxJsonForm
      :schema="schema"
      v-model="form"
      :disable-collapse="disableCollapse"
      :disable-edit-json="disableEditJson"
      :disable-properties="disableProperties"
      :disable-title="disableTitle"
    />

    <button @click="setSchemaA">Schema A</button>
    <button @click="setSchemaB">Schema B</button>
    <button @click="setSchemaAValue">Schema A Value</button>
    <button @click="setSchemaBValue">Schema B Value</button>
    <button @click="disableCollapse = !disableCollapse">
      Toggle disableCollapse
    </button>
    <button @click="disableEditJson = !disableEditJson">
      Toggle disableEditJson
    </button>
    <button @click="disableProperties = !disableProperties">
      Toggle disableProperties
    </button>
    <button @click="disableTitle = !disableTitle">Hide Title</button>
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
      schema: null,
      form: {},
      disableCollapse: true,
      disableEditJson: true,
      disableProperties: true,
      disableTitle: false,
    };
  },
  methods: {
    setSchemaBValue() {
      this.form = { name: "SchemaA", age: 20, favorite_color: "#ffa500" };
    },
    setSchemaAValue() {
      this.form = {
        name: "Tim",
        age: 20,
        favorite_color: "#ffa500",
        gender: "male",
        date: "",
        location: {},
        pets: [{ type: "cat", name: "Walter" }],
      };
    },
    setSchemaA() {
      this.schema = {
        type: "object",
        required: [
          "name",
          "age",
          "date",
          "favorite_color",
          "gender",
          "location",
          "pets",
        ],
        properties: {
          name: {
            type: "string",
            description: "First and Last name",
            // minLength: 4,
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
          gender: {
            type: "string",
            enum: ["male", "female", "other"],
          },
          date: {
            type: "string",
            format: "date",
            options: {
              flatpickr: {},
            },
          },
          location: {
            type: "object",
            title: "Location",
            properties: {
              city: {
                type: "string",
                default: "San Francisco",
              },
              state: {
                type: "string",
                default: "CA",
              },
              citystate: {
                type: "string",
                description:
                  "This is generated automatically from the previous two fields",
                template: "{{city}}, {{state}}",
                watch: {
                  city: "location.city",
                  state: "location.state",
                },
              },
            },
          },
          pets: {
            type: "array",
            format: "table",
            title: "Pets",
            uniqueItems: true,
            items: {
              type: "object",
              title: "Pet",
              properties: {
                type: {
                  type: "string",
                  enum: ["cat", "dog", "bird", "reptile", "other"],
                  default: "dog",
                },
                name: {
                  type: "string",
                },
              },
            },
            default: [
              {
                type: "dog",
                name: "Walter",
              },
            ],
          },
        },
      };
    },
    setSchemaB() {
      this.schema = {
        type: "object",
        // required: ["name", "age", "favorite_color"],
        properties: {
          name: {
            type: ["string", "null"],
            description: "First and Last name",
            minLength: 1,
          },
          age: {
            type: ["integer", "null"],
            minimum: 18,
            maximum: 99,
          },
          favorite_color: {
            type: ["string", "null"],
            format: "color",
            title: "favorite color",
          },
        },
      };
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
