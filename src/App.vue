<template>
  <div id="app">
    <input v-model="userInput" placeholder="Введіть номер будинку" />
    <button @click="checkNumber">Перевірити</button>

    <building-validator
        v-for="(item, index) in paginatedBuildings"
        :key="index"
        :building-number="item"
    ></building-validator>

    <div>
      <button @click="prevPage" :disabled="currentPage === 1">Попередня сторінка</button>
      <button @click="nextPage" :disabled="currentPage === totalPages">Наступна сторінка</button>
      <span>Сторінка {{ currentPage }} з {{ totalPages }}</span>
    </div>
  </div>
</template>

<script>
import BuildingValidator from "@/components/BuildingValidator";

export default {
  name: 'App',
  components: {
    BuildingValidator,
  },
  data() {
    return {
      buildingNumbers: [],
      userInput: '',
      itemsPerPage: 1000,
      currentPage: 1,
    };
  },
  computed: {
    paginatedBuildings() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.buildingNumbers.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.buildingNumbers.length / this.itemsPerPage);
    },
  },
  methods: {
    checkNumber() {
      const isValid = /^[0-9/-]+$/.test(this.userInput);
      alert(isValid ? 'Номер валідний' : 'Номер невалідний');
    },
    async loadBuildingNumbers() {
      try {
        const response = await fetch('/buildingNumbers.txt');
        const text = await response.text();
        this.buildingNumbers = text.split('\n').filter(Boolean);
      } catch (error) {
        console.error('Помилка завантаження даних:', error);
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage -= 1;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage += 1;
      }
    },
  },
  mounted() {
    this.loadBuildingNumbers();
  },
};
</script>

<style>
#app {
  margin: 20px;
}
</style>
