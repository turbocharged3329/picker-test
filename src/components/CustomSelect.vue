<template>
  <div class="select">
    <div class="select__wrapper">
      <div
        class="select__input-wrapper"
        :class="{
          date: type == 'date',
        }"
      >
        <input
          ref="input"
          type="text"
          class="select__input"
          @pointerdown="togglePicker(!isPickerShown)"
          readonly
          :value="value"
          :class="{ active: isPickerShown, empty: !value }"
        />
        <button class="select__input-clear-btn" v-if="type == 'date'"></button>
        <button
          class="select__input-toggle-btn"
          v-if="type != 'date'"
          :class="{ opened: isPickerShown }"
          @pointerdown="togglePicker(!isPickerShown)"
        ></button>
      </div>
      <div
        class="single-choice__picker picker"
        v-if="isPickerShown && type == 'single-choice'"
        :class="selectData.length <= 7 ? 'small' : 'big'"
      >
        <template v-for="item in selectData">
          <div
            class="single-choice__picker-item"
            :class="{ selected: item.name == singleSelected }"
            :key="item.id"
            @pointerdown="selectItem(item.name)"
          >
            {{ item.name }}
          </div>
        </template>
      </div>
      <div
        class="multiple-choice__picker picker"
        v-if="isPickerShown && type == 'multiple-choice'"
      >
        <template v-for="item in selectData">
          <div
            class="multiple-choice__picker-item"
            :key="item.id"
            @pointerdown.stop="selectMultipleItem($event, item.name)"
          >
            {{ item.name }}
            <input
              class="custom-checkbox"
              :id="'input' + item.id"
              type="checkbox"
              :value="item.name"
              v-model="multipleSelected"
              @change="$emit('input', multipleSelected.length ? multipleSelected.join('; ') : [])"
            />
            <label :for="'input' + item.id"></label>
          </div>
        </template>
      </div>
      <div class="date__picker picker" v-if="isPickerShown && type == 'date'">
        <div class="date__picker-wrapper">
          <div class="date__picker-navbar">
            <button
              class="date__picker-navbar-btn decrease"
              @pointerdown="changeYear(false)"
            ></button>
            <span>{{ selectedDate.year }}</span>
            <button
              class="date__picker-navbar-btn increase"
              @pointerdown="changeYear"
            ></button>
          </div>
          <div class="date__picker-months-list">
            <template v-for="month in months">
              <div
                class="date__picker-months-item"
                :class="{ selected: selectedDate.month == month.name }"
                :key="month.id"
                @pointerdown="selectDate(month.name)"
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
    placeholder: {
      type: String,
      default: "Select",
    },
  },
  data() {
    return {
      singleSelected: null,
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
  computed: {
    selectedDateValue() {
      return `${this.selectedDate.month} ${this.selectedDate.year}`;
    },
  },
  methods: {
    togglePicker(toOpen = true) {
      this.isPickerShown = toOpen;
    },
    selectItem(itemData) {
      this.singleSelected = itemData;
      this.togglePicker(false);
      this.$emit("input", this.singleSelected);
    },
    selectDate(month) {
      this.selectedDate.month = month;
      this.$emit("input", this.selectedDateValue);
    },
    selectMultipleItem(event, itemData) {
      if (event.target.tagName != "LABEL") {
        let multipleInString = this.multipleSelected.join(",");

        if (!multipleInString.includes(itemData)) {
          multipleInString += itemData + ",";
          this.multipleSelected = multipleInString.split(",");
        } else {
          // const regex = new RegExp(`${itemData}; `);
          // multipleInString = multipleInString.replace(regex, '');
          // this.multipleSelected = multipleInString.split(",");
          this.multipleSelected.splice(
            this.multipleSelected.findIndex(elem => elem == itemData),
            1
          )
          if (!this.multipleSelected.length) {
            this.multipleSelected = []
          }
        }

        this.$emit("input", this.multipleSelected.join("; "));
      }
    },
    changeYear(increase = true) {
      increase ? this.selectedDate.year++ : this.selectedDate.year--;
    },
  },
  mounted() {
    if (this.placeholder) {
      this.$refs.input.value = this.placeholder;
    }
  },
};
</script>

<style lang="scss" scoped>
//общие стили
button {
  background: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
}
.picker {
  background: white;
  box-shadow: 0px 1px 4px rgba(181, 158, 140, 0.04),
    0px 4px 16px rgba(75, 64, 57, 0.1);
  border-radius: 3px;
  top: calc(100% + 10px);
  position: absolute;
}
.selected {
  background-color: #ff9a4d;
  color: white;
  &:hover {
    background-color: #ff9a4d !important;
    color: white;
  }
}
.select-item {
  flex-basis: 100%;
  height: 44px;
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-start;
  align-items: center;
  padding-left: 16px;
  &:hover {
    background: #fff9f4;
  }
  &:first-of-type {
    border-radius: 3px 3px 0px 0px;
  }
  &:last-of-type {
    border-radius: 0px 0px 3px 3px;
  }
}

.custom-checkbox {
  position: absolute;
  z-index: -1;
  opacity: 0;
  margin: 0px;
  & + label {
    display: inline-flex;
    align-items: center;
    user-select: none;
    &::before {
      content: "";
      display: inline-block;
      width: 1em;
      height: 1em;
      flex-shrink: 0;
      flex-grow: 0;
      border: 1.25px solid #d0d9de;
      border-radius: 2.5px;
      background-repeat: no-repeat;
      background-position: center center;
      background-size: 50% 50%;
    }
  }
  &:checked + label::before {
    border: 1px solid #ff9a4d;
    background-color: #ff9a4d;
    background-image: url("data:image/svg+xml,%3Csvg width='14' height='12' viewBox='0 0 14 12' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0.75 5.70041L5.4375 9.75L13.25 1' stroke='white' stroke-width='1.875'/%3E%3C/svg%3E%0A");
  }
}
//
.select {
  &__wrapper {
    position: relative;
  }
  &__input {
    cursor: pointer;
    min-width: 320px;
    height: 44px;
    box-sizing: border-box;
    border-radius: 3px;
    border: 1px solid silver;
    padding: 0px 50px 0 16px;
    &.active {
      border: 0.7px solid #ff9a4d;
      outline: none;
    }
    &.empty {
      color: #90a4af;
    }
    &:focus {
      outline: none;
    }
    &-wrapper {
      display: block;
      width: fit-content;
      height: fit-content;
      position: relative;
      &.date {
        position: relative;
        & .select__input {
          padding: 0 36px 0 42px;
        }
        &::before {
          content: "";
          width: 16px;
          height: 16px;
          position: absolute;
          top: 50%;
          left: 16px;
          transform: translateY(-50%);
          background-image: url("data:image/svg+xml,%3Csvg width='16' height='16' viewBox='0 0 16 16' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cg clip-path='url(%23clip0_2_147)'%3E%3Cpath d='M13.6364 0C14.2632 0 14.8644 0.249025 15.3077 0.692293C15.751 1.13556 16 1.73676 16 2.36364V13.6364C16 14.2632 15.751 14.8644 15.3077 15.3077C14.8644 15.751 14.2632 16 13.6364 16L2.36364 16C1.73676 16 1.13556 15.751 0.692293 15.3077C0.249025 14.8644 0 14.2632 0 13.6364L0 2.36364C0 1.73676 0.249025 1.13556 0.692293 0.692293C1.13556 0.249025 1.73676 0 2.36364 0L13.6364 0ZM14.9091 4.72945L1.09091 4.72945L1.09091 13.6364C1.09091 14.3389 1.66109 14.9091 2.36364 14.9091L13.6364 14.9091C13.9739 14.9091 14.2976 14.775 14.5363 14.5363C14.775 14.2976 14.9091 13.9739 14.9091 13.6364L14.9091 4.72945ZM4.18109 10.5462C4.4222 10.5462 4.65343 10.642 4.82392 10.8124C4.9944 10.9829 5.09018 11.2142 5.09018 11.4553C5.09018 11.6964 4.9944 11.9276 4.82392 12.0981C4.65343 12.2686 4.4222 12.3644 4.18109 12.3644C3.93998 12.3644 3.70875 12.2686 3.53827 12.0981C3.36778 11.9276 3.272 11.6964 3.272 11.4553C3.272 11.2142 3.36778 10.9829 3.53827 10.8124C3.70875 10.642 3.93998 10.5462 4.18109 10.5462ZM8.00218 10.5462C8.24329 10.5462 8.47452 10.642 8.64501 10.8124C8.81549 10.9829 8.91127 11.2142 8.91127 11.4553C8.91127 11.6964 8.81549 11.9276 8.64501 12.0981C8.47452 12.2686 8.24329 12.3644 8.00218 12.3644C7.76108 12.3644 7.52985 12.2686 7.35936 12.0981C7.18887 11.9276 7.09309 11.6964 7.09309 11.4553C7.09309 11.2142 7.18887 10.9829 7.35936 10.8124C7.52985 10.642 7.76108 10.5462 8.00218 10.5462ZM4.18109 6.90982C4.4222 6.90982 4.65343 7.0056 4.82392 7.17609C4.9944 7.34657 5.09018 7.5778 5.09018 7.81891C5.09018 8.06002 4.9944 8.29125 4.82392 8.46173C4.65343 8.63222 4.4222 8.728 4.18109 8.728C3.93998 8.728 3.70875 8.63222 3.53827 8.46173C3.36778 8.29125 3.272 8.06002 3.272 7.81891C3.272 7.5778 3.36778 7.34657 3.53827 7.17609C3.70875 7.0056 3.93998 6.90982 4.18109 6.90982ZM8.00218 6.90982C8.24329 6.90982 8.47452 7.0056 8.64501 7.17609C8.81549 7.34657 8.91127 7.5778 8.91127 7.81891C8.91127 8.06002 8.81549 8.29125 8.64501 8.46173C8.47452 8.63222 8.24329 8.728 8.00218 8.728C7.76108 8.728 7.52985 8.63222 7.35936 8.46173C7.18887 8.29125 7.09309 8.06002 7.09309 7.81891C7.09309 7.5778 7.18887 7.34657 7.35936 7.17609C7.52985 7.0056 7.76108 6.90982 8.00218 6.90982ZM11.824 6.90982C12.0651 6.90982 12.2963 7.0056 12.4668 7.17609C12.6373 7.34657 12.7331 7.5778 12.7331 7.81891C12.7331 8.06002 12.6373 8.29125 12.4668 8.46173C12.2963 8.63222 12.0651 8.728 11.824 8.728C11.5829 8.728 11.3517 8.63222 11.1812 8.46173C11.0107 8.29125 10.9149 8.06002 10.9149 7.81891C10.9149 7.5778 11.0107 7.34657 11.1812 7.17609C11.3517 7.0056 11.5829 6.90982 11.824 6.90982ZM13.6364 1.09091L2.36364 1.09091C2.02609 1.09091 1.70237 1.225 1.46368 1.46368C1.225 1.70237 1.09091 2.02609 1.09091 2.36364L1.09091 3.63855L14.9091 3.63855V2.36364C14.9091 2.02609 14.775 1.70237 14.5363 1.46368C14.2976 1.225 13.9739 1.09091 13.6364 1.09091Z' fill='%23FF9A4D' stroke='%23FF9A4D' stroke-width='0.5'/%3E%3C/g%3E%3Cdefs%3E%3CclipPath id='clip0_2_147'%3E%3Crect width='16' height='16' fill='white' transform='matrix(1 0 0 -1 0 16)'/%3E%3C/clipPath%3E%3C/defs%3E%3C/svg%3E%0A");
          background-position: center;
          background-size: contain;
          background-repeat: no-repeat;
          z-index: 1;
        }
      }
    }
    &-clear-btn {
      width: 10px;
      height: 10px;
      background-image: url("data:image/svg+xml,%3Csvg width='10' height='10' viewBox='0 0 10 10' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M10 0.875L9.125 0L5 4.125L0.875 0L0 0.875L4.125 5L0 9.125L0.875 10L5 5.875L9.125 10L10 9.125L5.875 5L10 0.875Z' fill='%2390A4AF'/%3E%3C/svg%3E%0A");
      background-position: center;
      background-size: cover;
      background-repeat: no-repeat;
      position: absolute;
      top: 50%;
      right: 16px;
      transform: translateY(-50%);
    }
    &-toggle-btn {
      width: 24px;
      height: 24px;
      background-image: url("data:image/svg+xml,%3Csvg width='12' height='7' viewBox='0 0 12 7' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11.7071 6.70711C11.3166 7.09763 10.6834 7.09763 10.2929 6.70711L6 2.41421L1.70711 6.70711C1.31658 7.09763 0.683417 7.09763 0.292893 6.70711C-0.0976315 6.31658 -0.0976315 5.68342 0.292893 5.29289L5.29289 0.292893C5.68342 -0.097631 6.31658 -0.097631 6.70711 0.292893L11.7071 5.29289C12.0976 5.68342 12.0976 6.31658 11.7071 6.70711Z' fill='%23253238'/%3E%3C/svg%3E%0A");
      background-repeat: no-repeat;
      background-size: auto;
      background-position: 100% center;
      transform: translateY(-50%);
      position: absolute;
      z-index: 1;
      right: 16px;
      top: 50%;
      &.opened {
        background-position: 0 center;
        transform: translateY(-50%) rotate(180deg);
      }
    }
  }
}
.date {
  &__picker {
    width: 290px;
    height: 220px;
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
.single-choice {
  &__picker {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    align-items: center;
    max-width: 320px;
    width: 320px;
    &.small {
      height: fit-content;
    }
    &.big {
      height: 300px;
      overflow-y: auto;
      &::-webkit-scrollbar {
        width: 16px; /* ширина scrollbar */
      }
      &::-webkit-scrollbar-track {
        background: transparent; /* цвет дорожки */
      }
      &::-webkit-scrollbar-thumb {
        background-color: #d0d9de; /* цвет плашки */
        border-radius: 30px; /* закругления плашки */
        border: 4px solid white; /* padding вокруг плашки */
      }
      scrollbar-width: 16px; /* "auto" или "thin"  */
      scrollbar-color: #d0d9de transparent; /* плашка скролла и дорожка */
    }
    &-item {
      @extend .select-item;
    }
  }
}
.multiple-choice {
  &__picker {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    align-items: center;
    max-width: 320px;
    width: 320px;
    height: fit-content;
    &-item {
      @extend .select-item;
      padding-right: 16px;
      justify-content: space-between;
      &:hover {
        .custom-checkbox + label::before {
          border: 1.25px solid #ff9a4d;
        }
      }
    }
  }
}
</style>
