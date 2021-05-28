<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <CustomDropdown
      :items="states"
      item-class="foo"
      btn-class="foo"
      container-class="foo"
      placeholder="foobar"
      multiSelect
      searchable
      isChecked
      @elementsSelected="selectedFilters($event)"
      @hidden="close"
    >
      <template #btnText>
        show options &#9729;
      </template>
    </CustomDropdown>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import CustomDropdown from "./components/CustomDropdown.vue";

export default {
  name: "App",
  data() {
    return {
      states: [
        // {
        //   id: "CO",
        //   value: "CO1",
        //   name: "East South Central"
        // },
        // {
        //   id: "CO",
        //   value: "CO2",
        //   name: "West South Central"
        // },
        // {
        //   id: "CO",
        //   value: "CO3",
        //   name: "New England"
        // },
        // {
        //   id: "CO",
        //   value: "CO4",
        //   name: "South Atlantic"
        // },
        // {
        //   id: "CO",
        //   value: "CO5",
        //   name: "East North Central"
        // },
        // {
        //   id: "CO",
        //   value: "CO6",
        //   name: "Mid Atlantic"
        // },
        // {
        //   id: "CO",
        //   value: "CO7",
        //   name: "Alabama"
        // },
        {
          id: "CO",
          value: "CO8",
          name: "Arizona"
        },
        {
          id: "CO",
          value: "CO9",
          name: "Arkansas"
        },
        {
          id: "CO",
          value: "CO10",
          name: "California",
          disabled: true
        },
        {
          id: "CO",
          value: "CO11",
          name: "Colorado",
          checked: true
        }
      ]
    };
  },
  components: {
    CustomDropdown
    // HelloWorld
  },
  methods: {
    selectedFilters(e) {
      console.log(e);
    },
    close() {
      console.log("close");
    }
  }
};
</script>

<style>
.foo{
  background: chocolate;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
