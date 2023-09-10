<template>
  <v-app>
    <v-app-bar app color="primary" dark> </v-app-bar>
    <v-main>
      <v-container>
        <v-btn class="mb-4" @click="openDialog">Create a Meeting</v-btn>

        <!-- NOTE: This is for UI only, no integration yet -->
        <AppCalendar />

        <v-dialog v-model="dialog" persistent max-width="600px">
          <v-card>
            <MeetingForm
              :closeDialog="closeDialog"
              @meeting-list="handleMeetingCreated"
            />
          </v-card>
        </v-dialog>

        <h5>Sample Payload</h5>
        {{ JSON.stringify(meetingList) }}
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import MeetingForm from './components/templates/MeetingForm';
import AppCalendar from './components/AppCalendar.vue';

export default {
  name: 'App',

  components: {
    MeetingForm,
    AppCalendar,
  },

  data() {
    return {
      dialog: false,
      meetingList: null,
    };
  },
  methods: {
    openDialog() {
      this.dialog = true;
    },
    closeDialog() {
      this.dialog = false;
    },

    handleMeetingCreated(meetingList) {
      this.meetingList = meetingList;
    },
  },
};
</script>
