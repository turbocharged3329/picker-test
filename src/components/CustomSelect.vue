<template>
  <div class="select">
    <div class="select__wrapper">
      <input
        ref="input"
        type="text"
        class="select__input"
        @click="togglePicker"
        :value="value"
      />
      <!-- <div class="select__picker" v-if="isPickerShown && type != 'date'"> -->
      <!-- <template v-for="item in selectData">
          <div class="select__picker-item" :key="item.id" @click="selectItem(item)">{{ item.name }}</div>
        </template> -->
      <!-- <template v-for="item in selectData">
          <div class="select__picker-item" :key="item.id" @click="selectItem(item)">
            {{ item.name }}
            <input type="checkbox" :value="item.name" v-model="multipleSelected"/>
          </div>
        </template> -->
      <!-- </div> -->
      <div class="date__picker">
        <div class="date__picker-wrapper">
          <div class="date__picker-navbar">
            <button
              class="date__picker-navbar-btn decrease"
              @click="changeYear(false)"
            ></button>
            <span>{{ selectedDate.year }}</span>
            <button
              class="date__picker-navbar-btn increase"
              @click="changeYear"
            ></button>
          </div>
          <div class="date__picker-months-list">
            <template v-for="month in months">
              <div
                class="date__picker-months-item"
                :class="{ selected: selectedDate.month == month.name }"
                :key="month.id"
                @click="selectDate(month.name)"
              >
                {{ month.name }}
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CustomSelect",
  components: {},
  props: {
    value: {
      type: String,
    },
    type: {
      type: String,
      default: "single-choice",
    },
    selectData: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      selected: null,
      multipleSelected: [],
      selectedDate: {
        year: 2022,
        month: "",
      },
      isPickerShown: false,
      months: [
        { name: "Янв", id: 1 },
        { name: "Фев", id: 2 },
        { name: "Мар", id: 3 },
        { name: "Апр", id: 4 },
        { name: "Май", id: 5 },
        { name: "Июн", id: 6 },
        { name: "Июл", id: 7 },
        { name: "Авг", id: 8 },
        { name: "Сен", id: 9 },
        { name: "Окт", id: 10 },
        { name: "Ноя", id: 11 },
        { name: "Дек", id: 12 },
      ],
    };
  },
  methods: {
    togglePicker() {
      this.isPickerShown = !this.isPickerShown;
    },
    selectItem(itemData) {
      if (this.type == "single-choice") this.selected = itemData;
    },
    selectDate(month) {
      this.selectedDate.month = month;
      this.$emit("input", this.selectedDateValue);
    },
    changeYear(increase = true) {
      increase ? this.selectedDate.year++ : this.selectedDate.year--;
    },
  },
  computed: {
    selectedDateValue() {
      return `${this.selectedDate.month} ${this.selectedDate.year}`;
    },
  },
};
</script>

<style lang="scss" scoped>
.select {
  &__wrapper {
    position: relative;
  }
  &__input {
    cursor: pointer;
    width: 100px;
    height: 30px;
    border: 1px solid silver;
  }
  &__picker {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    align-content: flex-start;
    max-height: 100px;
    height: 100px;
    width: 150px;
    overflow-y: auto;
    position: absolute;
    left: 0;
    top: calc(100% + 5px);
    border: 1px solid silver;
    &-item {
      display: block;
      flex-basis: 100%;
      height: 15px;
      text-align: left;
      cursor: pointer;
    }
  }
}
.date {
  &__picker {
    width: 290px;
    height: 220px;
    background: white;
    box-shadow: 0px 1px 4px rgba(181, 158, 140, 0.04),
      0px 4px 16px rgba(75, 64, 57, 0.1);
    border-radius: 3px;
    &-wrapper {
      width: 100%;
      height: 100%;
      padding: 0 20px;
      box-sizing: border-box;
    }
    &-navbar {
      width: 100%;
      height: 45px;
      border-bottom: 0.7px solid #d0d9de;
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      align-items: center;
      &-btn {
        display: block;
        width: 24px;
        height: 24px;
        background-image: url("data:image/svg+xml,%3Csvg width='7' height='13' viewBox='0 0 7 13' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M6.70711 0.792893C7.09763 1.18342 7.09763 1.81658 6.70711 2.20711L2.41421 6.5L6.70711 10.7929C7.09763 11.1834 7.09763 11.8166 6.70711 12.2071C6.31658 12.5976 5.68342 12.5976 5.29289 12.2071L0.292893 7.20711C-0.097631 6.81658 -0.097631 6.18342 0.292893 5.79289L5.29289 0.792893C5.68342 0.402369 6.31658 0.402369 6.70711 0.792893Z' fill='%23253238'/%3E%3C/svg%3E%0A");
        background-repeat: no-repeat;
        background-position: center;
        background-size: auto;
        border: none;
        background-color: transparent;
        padding: 0;
        &.increase {
          transform: rotate(180deg);
        }
      }
    }
    &-months {
      &-list {
        display: grid;
        grid-template-rows: repeat(4, 27px);
        grid-template-columns: repeat(3, 1fr);
        row-gap: 10px;
        column-gap: 15px;
        padding: 15px 0;
        height: 100%;
      }
      &-item {
        flex-basis: 33.33%;
        display: flex;
        flex-flow: row nowrap;
        justify-content: center;
        align-items: center;
        border-radius: 3px;
        cursor: pointer;
        &.selected {
          background-color: #ff9a4d;
          color: white;
          &:hover {
            background-color: #ff9a4d;
            color: white;
          }
        }
        &:hover {
          background: #fff9f4;
        }
      }
    }
  }
}
</style>
