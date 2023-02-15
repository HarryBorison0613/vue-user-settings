<template>
  <div class="grid grid-cols-12 gap-6">
    <div class="col-span-12">
      <label class="block text-gray-600">New Password</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="password" v-model="formData.password" type="password" class="input-field w-full" name="password"
          :class="{ 'has-error': isInvalid('password') }">
        <div v-if="isInvalid('password')" class="error-message">{{ errorMessage('password') }}</div>
      </div>
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

export default {
  name: "ChangePassword",
  mixins: [serverValidationErrorsMixin],
  components: {
    LoadingSpinner,
  },

  data() {
    return {
      loading: false,
      formData: {},
    }
  },

  methods: {
    saveChanges() {
      if (this.loading) return;
      this.loading = true;

      setTimeout(() => {
        axios
          .post(`/user-settings/change-password`, this.formData)
          .then((response) => {
            $notify.success({
              title: 'Success',
              message: 'Submitted successfully',
              showClose: true,
            })
          })
          .catch((error) => {
            console.log(error, 'AppLeadEditForm - submit()')
            console.log(error.response)
            this.errors = error.response.data.errors;
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