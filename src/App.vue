<template>
  <div id="app" class="container">
    <NxJsonForm :schema="schema" v-model="form" />

    <button @click="setSchemaA">Schema A</button>
    <button @click="setSchemaB">Schema B</button>
    <button @click="setSchemaAValue">Schema A Value</button>
    <button @click="setSchemaBValue">Schema B Value</button>
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

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
