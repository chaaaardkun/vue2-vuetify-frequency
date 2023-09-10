<template>
  <v-container>
    <h4>Select Weekdays</h4>
    <v-card-text>
      <div class="week-checkboxes">
        <v-checkbox
          v-for="day in weekdays"
          :key="day"
          v-model="selectedDays"
          :label="day"
          :value="day"
          color="primary"
          hide-details
          class="circular-checkbox"
          @change="emitSelectedDays"
        />
      </div>
    </v-card-text>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      weekdays: ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'],
      selectedDays: [],
    };
  },
  methods: {
    emitSelectedDays() {
      this.$emit('update:selected-days', this.selectedDays);
    },
  },
};
</script>

<style scoped>
.week-checkboxes {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}
</style>
