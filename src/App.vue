<template>
  <div id="app">
    <p>{{ selected }}</p>
    <custom-select
      :select-data="selectData"
      :type="'multiple-choice'"
      v-model="selected"
    ></custom-select>
  </div>
</template>

<script>
import CustomSelect from "@/components/CustomSelect.vue";
export default {
  name: "App",
  components: {
    CustomSelect,
  },
  data() {
    return {
      selectData: [
        {
          id: 1,
          name: "123",
        },
        {
          id: 2,
          name: "345",
        },
        {
          id: 3,
          name: "678",
        },
        {
          id: 4,
          name: "910",
        },
        {
          id: 5,
          name: "302",
        },
        {
          id: 6,
          name: "3215",
        },
        {
          id: 7,
          name: "11215",
        },
        {
          id: 8,
          name: "1999",
        },
      ],
      selected: null,
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
