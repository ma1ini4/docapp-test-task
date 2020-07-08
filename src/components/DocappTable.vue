<template>
    <div>
        <table class="table">
            <thead>
            <tr>
                <th><abbr title="Room and appointment code">#</abbr></th>
                <th>Full name</th>
                <th>Room status</th>
                <th>Appointment date</th>
                <th>Waiting time</th>
                <th>Doctor's name</th>
                <th>Assistant's name</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="room in rooms" :key="room.code" @click="openModal(room)">
                <th>Room {{ room.code }} ({{ room.appointment.code }})</th>
                <th>{{ room.appointment.first_name }} {{ room.appointment.last_name }}</th>
                <th>{{ room.status.title }}</th>
                <th>{{ Moment(room.appointment.start_time, 'hh:mm A').format('hh:mm A') }}</th>
                <th>{{ remainingTime(room.appointment.start_time) }}</th>
                <th>{{ room.appointment.doctor_title }}</th>
                <th>{{ room.appointment.assistant }}</th>
            </tr>
            </tbody>
        </table>
        <docapp-modal
            :currentRoom="currentRoom"
            :modalIsActive="modalIsActive"
            @modalClosed="modalIsActive = false"
        />
    </div>
</template>

<script>
import rooms from '@/../data/rooms';
import Moment from 'moment';
import MomentCountdown from 'moment-countdown'
import DocappModal from "@/components/DocappModal";

export default {
  name: 'DocappTable',
  components: {
    DocappModal
  },
  data () {
    return {
      rooms,
      Moment: Moment,
      currentRoom: null,
      modalIsActive: false,
      selectedConsentForms: null
    }
  },
  methods: {
    remainingTime(date) {
      return Moment(date, 'hh:mm A').countdown();
    },
    openModal(room) {
      this.currentRoom = room;
      this.modalIsActive = true;
    }
  }
}
</script>