<template>
  <div id="app">
    <p>{{ selected }}</p>
    <custom-select :select-data="selectData" v-model="selected"></custom-select>
  </div>
</template>

<script>
import CustomSelect from '@/components/CustomSelect.vue';
export default {
  name: 'App',
  components: {
    CustomSelect
  },
  data() {
    return {
      selectData: [
        {
          id: 1,
          name: '123',
        },
        {
          id: 2,
          name: '345',
        }
      ],
      selected: null,
    }
  }
}
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
