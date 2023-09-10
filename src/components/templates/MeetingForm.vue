<template>
  <v-container>
    <h2 class="mb-4">Create a Meeting</h2>

    <div class="date-control--wrapper">
      <v-row>
        <v-col>
          <DateControl
            :selected-date="selectedStartDate"
            label="Start Date"
            :min-date="new Date().toISOString().split('T')[0]"
            @update:selected-date="selectedStartDate = $event"
          />
        </v-col>
        <v-col>
          <DateControl
            :selected-date="selectedEndDate"
            label="End Date"
            :min-date="selectedStartDate"
            @update:selected-date="selectedEndDate = $event"
          />
        </v-col>
      </v-row>
    </div>
    <v-select v-model="frequency" :items="frequencyOptions" label="Frequency" />

    <v-select
      v-if="frequency !== 'Never'"
      v-model="repeat"
      :items="repeatOptions"
      label="Repeat By"
    />

    <WeekPicker
      v-if="frequency === 'Weekly'"
      @update:selected-days="selectedDays = $event"
    />

    <v-text-field
      v-if="frequency === 'Weekly'"
      label="Week Reccurence"
      type="number"
      :value="weekReccurence"
      oninput="if(this.value < 0) this.value = 0;"
    />

    <TimeControl
      :repeat="repeat"
      :frequency="frequency"
      :timeList="timeList"
      :repeatRecurrence="repeatRecurrence"
      @update-time-list="updateTimeList"
      @update:repeat-recurrence="repeatRecurrence = $event"
    />

    <p class="mt-6">
      {{ frequency !== 'Never' ? frequency : '' }} Starting on
      {{ selectedStartDate }}, ending on
      {{ selectedEndDate }}
    </p>

    <v-card-actions>
      <v-spacer />
      <v-btn color="blue darken-1" text @click="closeModal"> Close </v-btn>
      <v-btn color="blue darken-1" text @click="createMeeting">
        Create Meeting
      </v-btn>
    </v-card-actions>
  </v-container>
</template>

<script>
import TimeControl from '@/components/templates/TimeControl.vue';
import DateControl from '@/components/templates/DateControl.vue';
import WeekPicker from '@/components/templates/WeekPicker.vue';

export default {
  props: {
    closeDialog: Function,
  },
  components: {
    TimeControl,
    DateControl,
    WeekPicker,
  },

  data() {
    const dateToday = new Date().toISOString().split('T')[0];
    return {
      selectedStartDate: dateToday,
      selectedEndDate: dateToday,

      frequency: 'Never',
      frequencyOptions: ['Daily', 'Weekly', 'Monthly', 'Yearly', 'Never'],

      repeat: 'Specific Time',
      repeatOptions: ['Specific Time', 'Time Increments'],
      repeatRecurrence: 0,

      weekReccurence: 0,
      selectedDays: [],

      timeList: [
        {
          startTime: '12:00',
          endTime: '08:00',
        },
      ],
    };
  },
  methods: {
    updateTimeList(updatedTimeList) {
      this.timeList = updatedTimeList;
    },
    closeModal() {
      this.closeDialog();
    },
    createMeeting() {
      // Handle meeting creation logic based on selected options
      const formData = {
        startDate: this.selectedStartDate,
        endDate: this.selectedEndDate,
        frequency: this.frequency,
        repeat: this.repeat,
        repeatReccurence: this.repeatRecurrence,
        weekReccurence: this.weekReccurence,
        selectedDays: this.selectedDays,
        timeList: this.timeList,
      };
      this.$emit('meeting-list', formData);
      this.closeDialog();
    },
  },
};
</script>
