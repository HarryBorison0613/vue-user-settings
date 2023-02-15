<template>
  <div class="grid grid-cols-12 gap-6">
    <div class="col-span-12">
      <el-switch v-model="formData.notify_missed_messages" id="notify_missed_messages" />
      <label for="notify_missed_messages" class="ml-2 text-sm text-gray-500">Missed Text Messages</label>
      <div class="info-message">
        If Enabled, TopBroker will send you a text message notifying of a new text message if unread within 15 minutes.
        This will be a notification only - you will not be able to reply directly from your mobile phone. Notifications
        are only sent from 10am to 9pm EST.
      </div>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_appointments_reminder" id="notify_appointments_reminder" />
      <label for="notify_appointments_reminder" class="ml-2 text-sm text-gray-500">Appointments Text Reminder</label>
      <div class="info-message">
        If Enabled, TopBroker will send you a text message notifying you of an upcoming appointment 15 minutes before
        the start time. This will be a notification only - you will not be able to reply directly from your mobile
        phone. Notifications are only sent from 9am to 9pm EST.
      </div>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_turning_65" id="notify_turning_65" />
      <label for="notify_turning_65" class="ml-2 text-sm text-gray-500">Turning 65</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_turning_26" id="notify_turning_26" />
      <label for="notify_turning_26" class="ml-2 text-sm text-gray-500">Turning 26</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_birthday" id="notify_birthday" />
      <label for="notify_birthday" class="ml-2 text-sm text-gray-500">Birthday</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_wedding_anniversary" id="notify_wedding_anniversary" />
      <label for="notify_wedding_anniversary" class="ml-2 text-sm text-gray-500">Wedding Anniversary</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_client_anniversary" id="notify_client_anniversary" />
      <label for="notify_client_anniversary" class="ml-2 text-sm text-gray-500">Client Anniversary</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_expiring_policy" id="notify_expiring_policy" />
      <label for="notify_expiring_policy" class="ml-2 text-sm text-gray-500">Expiring Policy Notification</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.notify_autoresponders" id="notify_autoresponders" />
      <label for="notify_autoresponders" class="ml-2 text-sm text-gray-500">Autoresponders ON/OFF</label>
    </div>

    <div class="col-span-12">
      <button @click.prevent="saveChanges" :disabled="loading" type="submit"
        class="mb-3 mt-3 btn btn-primary flex items-center justify-center text-sm"
        :class="{ 'pointer-events-none': loading }">
        <LoadingSpinner v-if="loading"></LoadingSpinner>
        <span v-else>Save changes</span>
      </button>
    </div>
  </div>
</template>

<script>
import LoadingSpinner from "@/Components/LoadingSpinner.vue";
import _ from "lodash";
import serverValidationErrorsMixin from "../mixins/serverValidationErrorsMixin";
import { ElSwitch } from "element-plus";

export default {
  name: "NotificationAlerts",
  mixins: [serverValidationErrorsMixin],
  components: {
    LoadingSpinner,
    ElSwitch
  },

  props: {
    userSettings: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      loading: false,
      formData: {},
    }
  },

  mounted() {
    this.formData.notify_missed_messages = this.userSettings.notify_missed_messages == 1 ? true : false
    this.formData.notify_appointments_reminder = this.userSettings.notify_appointments_reminder == 1 ? true : false
    this.formData.notify_turning_65 = this.userSettings.notify_turning_65 == 1 ? true : false
    this.formData.notify_turning_26 = this.userSettings.notify_turning_26 == 1 ? true : false
    this.formData.notify_birthday = this.userSettings.notify_birthday == 1 ? true : false
    this.formData.notify_wedding_anniversary = this.userSettings.notify_wedding_anniversary == 1 ? true : false
    this.formData.notify_client_anniversary = this.userSettings.notify_client_anniversary == 1 ? true : false
    this.formData.notify_expiring_policy = this.userSettings.notify_expiring_policy == 1 ? true : false
    this.formData.notify_autoresponders = this.userSettings.notify_autoresponders == 1 ? true : false
  },

  methods: {
    saveChanges() {
      if (this.loading) return;
      this.loading = true;

      setTimeout(() => {
        axios
          .put(`/user-settings/update-alert-settings`, this.formData)
          .then((response) => {
            this.$emit('sync-data');
            $notify.success({
              title: 'Success',
              message: 'Submitted successfully',
              showClose: true,
            })
          })
          .catch((error) => {
            console.log(error, 'AppLeadEditForm - submit()')
            console.log(error.response)
            $notify.error({
              title: 'Error',
              message: error.response.message ? error.response.message : 'There is an error occurred',
              showClose: true,
            })
          })
          .finally(() => {
            this.loading = false;
          })
      }, 500)
    },

    showNotification(message = null, title = 'Success') {
      let messageContent = message ? message : 'Submitted successfully';
      $notify.success({
        title: title,
        message: messageContent,
        showClose: true,
      })
    },
  },
}
</script>