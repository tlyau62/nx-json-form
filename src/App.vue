<template>
  <div id="app" class="container">
    <div>
      <h4>Form value</h4>
      <div>
        {{ form }}
      </div>
      <div>
        {{ valid }}
      </div>
    </div>

    <NxJsonForm
      :schema="schema"
      @validation="valid = $event"
      v-model="form"
      :disable-collapse="disableCollapse"
      :disable-edit-json="disableEditJson"
      :disable-properties="disableProperties"
      :disable-title="disableTitle"
      :show-opt-in="showOptIn"
    />

    <button @click="setSchemaA">Schema A</button>
    <button @click="setSchemaB">Schema B</button>
    <button @click="setSchemaC">Schema C</button>
    <button @click="setSchemaD">Schema D</button>
    <button @click="setSchemaAValue">Schema A Value</button>
    <button @click="setSchemaBValue">Schema B Value</button>
    <button @click="setSchemaCValue">Schema C Value</button>
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
      schema: {
        type: "object",
        title: "Car",
        required: ["make"],
        properties: {
          make: {
            type: "string",
            enum: ["Toyota", "BMW"],
          },
        },
      },
      form: {},
      disableCollapse: true,
      disableEditJson: true,
      disableProperties: true,
      disableTitle: false,
      showOptIn: true,
      valid: [],
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
    setSchemaCValue() {
      this.form = {};
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
    setSchemaC() {
      this.schema = {
        type: "object",
        title: "Car",
        properties: {
          make: {
            type: ["null", "string"],
            enum: ["Toyota", "BMW"],
          },
        },
      };
    },
    setSchemaD() {
      const regions = [
        {
          value: "CustomPage",
          label: "Custom Page",
        },
        {
          value: "Widget",
          label: "Widget",
        },
        {
          value: "BrowseFolder",
          label: "Browse Folder",
        },
        {
          value: "ViewPropertyValues",
          label: "View Property Values",
        },
        {
          value: "EditPropertyValues",
          label: "Edit Property Values",
        },
        {
          value: "SearchPropertyValues",
          label: "Search Property Values",
        },
      ];

      this.schema = {
        type: "object",
        properties: {
          Name: {
            type: "string",
          },
          Region: {
            type: "string",
            enum: regions.map((r) => r.value),
            options: {
              enum_titles: regions.map((r) => r.label),
            },
          },
          ViewType: {
            type: "string",
            enum: ["vue", "vue-compiled", "html", "razor"],
          },
          SourcePath: {
            type: "string",
            default: "",
          },
          DataProvider: {
            type: "string",
            enum: ["DummyTileDataProvider"],
            options: {
              dependencies: {
                ViewType: "razor",
              },
            },
          },
          ConfigJsonSchema: {
            type: "string",
            format: "json",
            options: {
              ace: {
                tabSize: 2,
                useSoftTabs: true,
                wrap: true,
              },
            },
            default: JSON.stringify({
              type: "object",
              properties: {
                number: { type: "number" },
                street_name: { type: "string" },
                street_type: {
                  type: "string",
                  enum: ["Street", "Avenue", "Boulevard"],
                },
              },
            }),
          },
          ConfigUIOptions: {
            type: "string",
            format: "json",
            options: {
              ace: {
                tabSize: 2,
                useSoftTabs: true,
                wrap: true,
              },
            },
          },
        },
        required: ["Name", "Region", "ViewType", "SourcePath"],
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
