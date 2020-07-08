<template>
    <div class="modal" :class="{'is-active': modalIsActive}" v-if="currentRoom">
        <div class="modal-background"></div>
        <div class="modal-content">
            <section class="modal-card-body">
                <div>
                    <ul>
                        <li>Room {{ currentRoom.code }}</li>
                        <li>Appointment Date {{ currentRoom.appointment.start_date }}</li>
                        <li>
                            Vital signs:
                            HT: {{ currentRoom.appointment.vital_signs.height_ft }}'{{ currentRoom.appointment.vital_signs.height_in }}''
                            WT: {{ currentRoom.appointment.vital_signs.weight }}lbs
                            BMI: {{ currentRoom.appointment.vital_signs.bmi }}
                        </li>
                        <li>
                            {{ getContraction(currentRoom.appointment.gender, currentRoom.appointment.is_doctor) }}
                            {{ currentRoom.appointment.first_name }}
                            {{ currentRoom.appointment.last_name }},
                            {{ getAgeByBirthday(currentRoom.appointment.birthday) }},
                            {{ currentRoom.appointment.gender[0] }}
                        </li>
                    </ul>
                </div>
                <div>
                    <div v-for="consentFrom in consentForms" :key="consentFrom.id">
                        <label class="checkbox" @input="pushToSelected(consentFrom)">
                            <input type="checkbox">
                            {{ consentFrom.title }}
                        </label>
                    </div>
                </div>
                <button class="button is-success" v-if="selectedConsentForms.length" @click="showForm = true">Sign</button>

                <docapp-tabs
                    :selected-consent-forms="selectedConsentForms"
                    :initials="currentRoom.appointment.first_name[0] + currentRoom.appointment.last_name[0]"
                    v-if="showForm"
                />
            </section>
        </div>
        <button class="modal-close is-large" aria-label="close" @click="closeModal"></button>
    </div>
</template>

<script>
import consentForms from '@/../data/consent-forms'
import Moment from 'moment';
import DocappTabs from "@/components/DocappTabs";
export default {
  name: 'DocappModal',
  components: {
    DocappTabs
  },
  props: {
    modalIsActive: {
      type: Boolean,
      default: false
    },
    currentRoom: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {
      Moment: Moment,
      consentForms,
      selectedConsentForms: [],
      showForm: false
    }
  },
  methods: {
    closeModal() {
      this.$emit('modalClosed');
    },
    getAgeByBirthday(birthday) {
      if (!birthday) return '';
      return Moment().diff(Moment(birthday), 'years');
    },
    getContraction(gender, role) {
      if (role) return 'Dr.';
      return gender === 'Male' ? 'Mr.' : 'Ms.'
    },
    pushToSelected(consentForm) {
      const isInArray = this.selectedConsentForms.find((selectedConsentForm) => {
        return selectedConsentForm === consentForm;
      });
      isInArray ? this.selectedConsentForms.splice(this.selectedConsentForms.indexOf(isInArray), 1) :this.selectedConsentForms.push(consentForm);
    }
  }
}
</script>