<template>
  <v-container>
    <v-row>
      <v-col>
        <v-switch
          label="Отображать пустые рубрики"
          v-model="showEmpty"
          @change="fetchRubrics"
          dense
        ></v-switch>

        <p class="selected-count">Сумма count-ов отмеченных рубрик: {{ selectedCountSum }}</p>

        <v-list dense class="compact-list">
          <RubricItem
            v-for="rubric in rubrics"
            :key="rubric.id"
            :rubric="rubric"
            :level="0"
            @toggleSelection="toggleSelection"
          />
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';
import RubricItem from './RubricItem.vue';

export default {
  name: 'RubricTree',
  components: { RubricItem },
  data() {
    return {
      rubrics: [],
      showEmpty: false,
      selectedRubrics: new Set(),
    };
  },
  computed: {
    selectedCountSum() {
      let total = 0;
      this.selectedRubrics.forEach(rubric => {
        total += rubric.totalCount;
      });
      return total;
    },
  },
  methods: {
    async fetchRubrics() {
      const allowEmpty = this.showEmpty ? 1 : 0;
      const { data } = await axios.get(`https://www.klerk.ru/yindex.php/v3/event/rubrics?allowEmpty=${allowEmpty}`);
      this.rubrics = this.calculateTotalCounts(data);
    },
    calculateTotalCounts(rubrics) {
      return rubrics.map(rubric => {
        rubric.totalCount = rubric.count + (rubric.children ? this.calculateTotalCounts(rubric.children).reduce((sum, child) => sum + child.totalCount, 0) : 0);
        return rubric;
      });
    },
    toggleSelection(rubric) {
      if (this.selectedRubrics.has(rubric)) {
        this.selectedRubrics.delete(rubric);
      } else {
        this.selectedRubrics.add(rubric);
      }
    },
  },
  created() {
    this.fetchRubrics();
  },
};
</script>

<style scoped>
.selected-count {
  font-size: 0.9rem;
  color: gray;
  margin: 8px 0;
}
.compact-list {
  padding: 0;
}
</style>
