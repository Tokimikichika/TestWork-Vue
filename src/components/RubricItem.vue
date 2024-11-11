<template>
  <v-list-item class="compact-item">
    <v-list-item-action>
      <v-checkbox 
        v-model="isChecked" 
        @click.stop="toggleCheckbox" 
        dense
      ></v-checkbox>
    </v-list-item-action>

    <v-list-item-content>
      <v-list-item-title class="title">
        <v-btn icon @click="toggleCollapse" small>
          <v-icon size="18">{{ isCollapsed ? 'mdi-chevron-right' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
        <a :href="'https://www.klerk.ru' + rubric.url" class="rubric-link">{{ rubric.title }}</a>
        <span class="count-text">({{ rubric.count }} / {{ rubric.totalCount }})</span>
      </v-list-item-title>
    </v-list-item-content>

    <v-list
      v-if="!isCollapsed && rubric.children && rubric.children.length"
      dense
      class="compact-children"
    >
      <RubricItem
        v-for="child in rubric.children"
        :key="child.id"
        :rubric="child"
        :level="level + 1"
        @toggleSelection="$emit('toggleSelection', $event)"
      />
    </v-list>
  </v-list-item>
</template>

<script>
export default {
  name: 'RubricItem',
  props: {
    rubric: Object,
    level: Number,
  },
  data() {
    return {
      isCollapsed: false,
      isChecked: false,  // Значение чекбокса
    };
  },
  methods: {
    toggleCollapse() {
      this.isCollapsed = !this.isCollapsed;
    },
    toggleCheckbox() {
      this.isChecked = !this.isChecked;  // Изменяем состояние чекбокса при клике
      this.$emit('toggleSelection', this.rubric);  // Передаем состояние родительскому компоненту
    },
  },
};
</script>

<style scoped>
.compact-item {
  padding: 0 4px;
}
.title {
  display: flex;
  align-items: center;
  font-size: 0.9rem;
}
.rubric-link {
  margin-left: 4px;
  text-decoration: none;
  color: inherit;
}
.count-text {
  margin-left: 8px;
  font-size: 0.8rem;
  color: gray;
}
.compact-children {
  padding-left: 16px;
}
</style>
