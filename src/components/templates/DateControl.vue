<template>
  <v-menu
    v-model="menuVisible"
    :close-on-content-click="false"
    transition="scale-transition"
    offset-y
    min-width="auto"
  >
    <template v-slot:activator="{ on }">
      <v-text-field :value="selectedDate" :label="label" readonly v-on="on" />
    </template>
    <v-date-picker
      v-model="localSelectedDate"
      @input="updateSelectedDate"
      :min="minDate"
    />
  </v-menu>
</template>

<script>
export default {
  props: {
    selectedDate: String,
    label: String,
    minDate: String,
  },
  data() {
    return {
      menuVisible: false,
      localSelectedDate: this.selectedDate,
    };
  },
  methods: {
    updateSelectedDate(newValue) {
      this.localSelectedDate = newValue;
      this.$emit('update:selected-date', newValue);
    },
  },
  watch: {
    selectedDate(newValue) {
      this.localSelectedDate = newValue;
    },
  },
};
</script>
