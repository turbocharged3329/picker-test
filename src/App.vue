<template>
  <div id="app">
    <p>{{ selected }}</p>
    <custom-select
      :select-data="selectData"
      :type="'multiple-choice'"
      :placeholder="'Select me again'"
      :label="'Date1'"
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
      //массив вариантов выбора для селектов типов "single-choice" и "multiple-choice"
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
      //выбранное в селекте значение
      selected: null,
    };
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@200;300;400;600;700&display=swap');
html {
  width: 100vw;
  height: 100vh;
}
#app {
  font-family: 'Nunito Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
input {
  font-family: 'Nunito Sans', sans-serif;
}
</style>
