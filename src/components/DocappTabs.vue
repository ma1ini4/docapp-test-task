<template>
    <div>
        <div class="tabs" v-if="selectedConsentForms && selectedConsentForms.length">
            <ul>
                <li
                        v-for="selectedConsentForm in selectedConsentForms"
                        :key="selectedConsentForm.id"
                        @click="selectedTabId = selectedConsentForm.id"
                        :class="{'is-active': selectedTabId === selectedConsentForm.id}">
                    <a>{{ selectedConsentForm.short_title }}</a>
                </li>
            </ul>
        </div>
        <section>
            <ul>
                <li v-for="item in content">
                    <label class="checkbox" v-if="item.need_initials">
                        <input type="checkbox" v-model="item.selected" v-if="!item.selected">
                        <span class="is-bold" v-else>{{ initials }} |</span>
                    </label>
                    {{ item.content }}
                </li>
            </ul>
        </section>
    </div>
</template>

<script>
export default {
  name: 'DocappTabs',
  props: {
    selectedConsentForms: {
      type: Array,
      default: () => {
        return []
      }
    },
    initials: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      selectedTabId: 0,
      content: []
    }
  },
  computed: {
    formContent() {
      if (this.selectedConsentForms && this.selectedConsentForms.length) {
        const find = this.selectedConsentForms.find((form) => {
          return form.id === this.selectedTabId;
        });
        const consentFromContent = require(`@/../data/consent-form-details/${find.code}`);
        this.content =  consentFromContent.map((form) => {
          form.selected = false;
          return form;
        })
      }
    }
  },
  methods: {
    selectTab() {

    }
  },
  created() {
    console.log(this.selectedConsentForms);
  }
}
</script>