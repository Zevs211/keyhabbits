<template>
  <div id="app">
    <chart :options="chartOptions" />
    <div class="inline">
      <div class="seed-input">
        <v-input
          :isNotValid="isNotValidName"
          inputLabel="Введите название"
          v-model.lazy="seedName"
        />
      </div>
      <v-input
        class="ml"
        inputLabel="Введите данные: 0, 0, 0, 0,"
        v-model.lazy="seedValues"
      />
      <v-button
        class="ml"
        @click.native="onSeedAdd"
        :disabled="isSubmitDisabled"
      >
        Добавить вид семян
      </v-button>
    </div>
    <v-button class="random-btn mt" @click.native="randomizeSeed"
      >Задать случайное значение</v-button
    >
  </div>
</template>

<script>
import { Chart } from "highcharts-vue";
import VButton from "./components/VButton.vue";
import VInput from "./components/VInput.vue";
import MockedData from "@/mocks/mocked-data.json";

export default {
  components: {
    Chart,
    VButton,
    VInput,
  },
  data: () => ({
    chartOptions: {
      chart: {
        type: "area",
        zoomType: "x",
        panning: true,
        panKey: "shift",
      },
      boost: {
        useGPUTranslations: true,
      },
      title: {
        text: "Урожайность семян на Луне",
      },
      subtitle: {
        text: 'по мотивам книги "Незнайка на Луне"',
      },
      yAxis: {
        title: {
          text: "шт./м2",
        },
      },
      xAxis: {
        title: {
          text: "годы",
        },
        categories: [2025, 2026, 2027, 2028],
      },
      series: [
        {
          name: "Пшеница",
          data: [49, 48, 53, 43],
        },
        {
          name: "Гречиха",
          data: [33, 29, 36, 30],
        },
        {
          name: "Ячмень",
          data: [22, 13, 15, 16],
        },
      ],
    },
    selectedName: "",
    seedNameData: "",
    seedValues: "",
    isNotValidName: false,
    namesList: MockedData,
  }),
  computed: {
    isSubmitDisabled() {
      return (
        this.seedName === "" || this.seedValues === "" || this.isNotValidName
      );
    },
    seedName: {
      get() {
        return this.seedNameData;
      },
      set(value) {
        this.seedNameData = value;
        this.isNotValidName = this.chartOptions.series.some(
          (el) => el.name.toLowerCase() === value.toLowerCase()
        );
      },
    },
  },
  methods: {
    onSeedAdd() {
      const name = this.seedName.trim();
      const data = this.seedValues
        .trim()
        .split(",")
        .map((value) => +value)
        .slice(0, 4);
      this.chartOptions.series.push({
        name,
        data,
      });
      this.seedName = "";
      this.seedValues = "";
      this.isNotValid = false;
    },
    randomizeSeed() {
      this.randomName();
      this.seedValues = `${this.randomFromRange(
        0,
        100
      )}, ${this.randomFromRange(0, 100)}, ${this.randomFromRange(
        0,
        100
      )}, ${this.randomFromRange(0, 100)}`;
    },
    randomFromRange(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    randomName() {
      const name = this.namesList;
      const idx = Math.floor(Math.random() * this.namesList.length);
      this.selectedName = name[idx];
      this.seedName = this.selectedName.name;
    },
  },
};
</script>

<style lang="scss" scoped>
.inline {
  display: flex;
  margin-top: 32px;
}
.ml {
  margin-left: 16px;
}
.mt {
  margin-top: 16px;
}
</style>
