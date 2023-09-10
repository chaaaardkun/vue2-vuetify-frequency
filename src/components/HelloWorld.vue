<template>
  <v-app>
    <v-container>
      <v-card max-width="600" class="mx-auto">
        <v-card-title>Create a Meeting</v-card-title>

        <v-card-text>
          <div class="date-control--wrapper">
            <DateControl
              :selected-date="selectedStartDate"
              label="Select Start Date"
              :min-date="new Date().toISOString().split('T')[0]"
              @update:selected-date="selectedStartDate = $event"
            />

            <DateControl
              :selected-date="selectedEndDate"
              label="Select End Date"
              :min-date="selectedStartDate"
              @update:selected-date="selectedEndDate = $event"
            />
          </div>
          <v-select
            v-model="frequency"
            :items="frequencyOptions"
            label="Frequency"
          />

          <v-select
            v-if="frequency !== 'Never'"
            v-model="repeat"
            :items="repeatOptions"
            label="Repeat By"
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

          <v-btn dark color="green lighten-2" @click="createMeeting"
            >Create Meeting</v-btn
          >
        </v-card-text>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
import TimeControl from '@/components/templates/TimeControl.vue';
import DateControl from '@/components/templates/DateControl.vue';

export default {
  components: {
    TimeControl,
    DateControl,
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
    createMeeting() {
      // Handle meeting creation logic based on selected options
      const formData = {
        startDate: this.selectedStartDate,
        endDate: this.selectedEndDate,
        frequency: this.frequency,
        repeat: this.repeat,
        repeatReccurence: this.repeatRecurrence,
        timeList: this.timeList,
      };
      console.log(formData);
    },
  },
};
</script>
