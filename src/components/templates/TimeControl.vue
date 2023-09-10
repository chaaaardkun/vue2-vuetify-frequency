<template>
  <div>
    <v-row
      v-for="(time, index) in frequency !== 'Never' &&
      repeat !== 'Time Increments'
        ? localTimeList
        : localTimeList.slice(0, 1)"
      :key="index"
    >
      <v-col v-if="repeat === 'Time Increments'">
        <v-text-field
          label="Reccurence by Minutes"
          type="number"
          oninput="if(this.value < 0) this.value = 0;"
          :value="localRepeatReccurence"
          @input="updateReccurence"
          v-on="on"
        />
      </v-col>

      <v-col>
        <v-menu
          v-model="startTimeMenu[localTimeList?.indexOf(time)]"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="time.startTime"
              label="Select Start Time"
              v-on="on"
            />
          </template>
          <v-time-picker
            v-model="time.startTime"
            @input="!startTimeMenu[localTimeList?.indexOf(time)]"
          />
        </v-menu>
      </v-col>

      <v-col>
        <v-menu
          v-model="endTimeMenu[localTimeList?.indexOf(time)]"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="time.endTime"
              label="Select End Time"
              v-on="on"
            />
          </template>
          <v-time-picker
            v-model="time.endTime"
            @input="!endTimeMenu[localTimeList?.indexOf(time)]"
          />
        </v-menu>
      </v-col>
    </v-row>

    <v-btn
      v-if="frequency !== 'Never' && repeat !== 'Time Increments'"
      @click="addNewTimeField"
    >
      <v-icon dark left> mdi-plus </v-icon>Add new time</v-btn
    >
  </div>
</template>

<script>
export default {
  props: {
    timeList: Array,
    frequency: String,
    repeat: String,
    repeatRecurrence: Number,
  },
  data() {
    return {
      localTimeList: [...this.timeList], // Create a local copy of timeList
      localRepeatReccurence: this.repeatRecurrence,

      startTimeMenu: [],
      endTimeMenu: [],
    };
  },
  methods: {
    updateReccurence(newValue) {
      this.local = newValue;
      this.$emit('update:repeat-recurrence', newValue);
    },

    addNewTimeField() {
      // Add a new time field to the localTimeList
      this.localTimeList.push({
        startTime: '12:00',
        endTime: '08:00',
      });

      // Emit the updated localTimeList to the parent component
      this.$emit('update-time-list', this.localTimeList);
    },
  },
};
</script>
