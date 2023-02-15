<template>
  <div class="grid grid-cols-12 gap-6">
    <div class="col-span-12">
      <label class="block text-gray-600">First Name</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="first_name" v-model="formData.first_name" type="text" class="input-field w-full" name="first_name"
          :class="{ 'has-error': isInvalid('first_name') }">
        <div v-if="isInvalid('first_name')" class="error-message">{{ errorMessage('first_name') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Last Name</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="last_name" v-model="formData.last_name" type="text" class="input-field w-full" name="last_name"
          :class="{ 'has-error': isInvalid('last_name') }">
        <div v-if="isInvalid('last_name')" class="error-message">{{ errorMessage('last_name') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Email</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="email" v-model="formData.email" type="text" class="input-field w-full" name="email"
          :class="{ 'has-error': isInvalid('email') }">
        <div v-if="isInvalid('email')" class="error-message">{{ errorMessage('email') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Office Phone</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="office_phone" v-model="formData.office_phone" type="text" class="input-field w-full"
          name="office_phone" :class="{ 'has-error': isInvalid('office_phone') }">
        <div v-if="isInvalid('office_phone')" class="error-message">{{ errorMessage('office_phone') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Mobile Phone</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="mobile_phone" v-model="formData.mobile_phone" type="text" class="input-field w-full"
          name="mobile_phone" :class="{ 'has-error': isInvalid('mobile_phone') }">
        <div v-if="isInvalid('mobile_phone')" class="error-message">{{ errorMessage('mobile_phone') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">DOB</label>
      <div class="mt-1 text-sm text-gray-900">
        <el-date-picker id="my-birthday" style="width: 100% !important;" v-model="formData.birthday" type="date"
          placeholder="Pick a day" format="MMM DD, YYYY" value-format="YYYY-MM-DD" size="large" />
        <!-- <input id="birthday" v-model="formData.birthday" type="text" class="input-field w-full"
          name="birthday" :class="{ 'has-error': isInvalid('birthday') }"> -->
        <div v-if="isInvalid('birthday')" class="error-message">{{ errorMessage('birthday') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Time Zone</label>
      <div class="mt-1 text-sm text-gray-900" :class="{ 'has-error': isInvalid('time_zone') }">
        <select v-model="formData.time_zone" data-placeholder="Select timezone" class="tom-select input-field w-full"
          id="time_zone" name="time_zone">
          <option v-for="(timezone, i) in timezones" :key="timezone.id" :value="timezone.value">{{ timezone.label }}
          </option>
        </select>
        <div v-if="isInvalid('time_zone')" class="error-message">{{ errorMessage('time_zone') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Address</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="address" v-model="formData.address" type="text" class="input-field w-full" name="address"
          :class="{ 'has-error': isInvalid('address') }">
        <div v-if="isInvalid('address')" class="error-message">{{ errorMessage('address') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">City</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="city" v-model="formData.city" type="text" class="input-field w-full" name="city"
          :class="{ 'has-error': isInvalid('city') }">
        <div v-if="isInvalid('city')" class="error-message">{{ errorMessage('city') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">State</label>
      <div class="mt-1 text-sm text-gray-900" :class="{ 'has-error': isInvalid('state_id') }">
        <select v-model="formData.state_id" data-placeholder="Select state" class="tom-select input-field w-full"
          id="state_id" name="state_id">
          <option v-for="(state, i) in states" :key="state.id" :value="state.id">{{ state.name }}</option>
        </select>
        <div v-if="isInvalid('state_id')" class="error-message">{{ errorMessage('state_id') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <label class="block text-gray-600">Zip</label>
      <div class="mt-1 text-sm text-gray-900">
        <input id="zip" v-model="formData.zip" type="text" class="input-field w-full" name="zip"
          :class="{ 'has-error': isInvalid('zip') }">
        <div v-if="isInvalid('zip')" class="error-message">{{ errorMessage('zip') }}</div>
      </div>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.allow_access_agency_owner" id="allow_access_agency_owner" />
      <label for="allow_access_agency_owner" class="ml-2 text-sm text-gray-500">Allow my agency owner to access my
        account</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.allow_access_support" id="allow_access_support" />
      <label for="allow_access_support" class="ml-2 text-sm text-gray-500">Allow TopBroker Support to access my
        account</label>
    </div>
    <div class="col-span-12">
      <el-switch v-model="formData.two_factor_auth" id="two_factor_auth" />
      <label for="two_factor_auth" class="ml-2 text-sm text-gray-500">Two-Step Authentication</label>
      <div class="info-message">
        By enabling this feature, you will be required to enter a code that is texted to your cell phone
        EACH TIME you login. Only enable this feature if you can receive text messages at the mobile number you provided
        above</div>
    </div>

    <div class="col-span-12">
      <button @click.prevent="saveChanges" :disabled="loading" type="submit"
        class="ml-auto mb-3 btn btn-primary flex items-center justify-center text-sm"
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
import { ElDatePicker, ElSwitch } from "element-plus";

export default {
  name: "MyInformation",
  mixins: [serverValidationErrorsMixin],
  components: {
    LoadingSpinner,
    ElDatePicker,
    ElSwitch
  },

  props: {
    userInfo: {
      type: Object,
      required: true,
    },
    userSettings: {
      type: Object,
      required: true,
    },
    states: {
      type: Array,
      required: true,
    },
    timezones: {
      type: Array,
      required: true,
    },
  },

  emits: ['sync-data'],

  data() {
    return {
      loading: false,
      formData: {},
    }
  },

  mounted() {
    this.formData = _.cloneDeep(this.userInfo)
    this.formData.allow_access_agency_owner = this.userSettings.allow_access_agency_owner == 1 ? true : false
    this.formData.allow_access_support = this.userSettings.allow_access_support == 1 ? true : false
    this.formData.two_factor_auth = this.userSettings.two_factor_auth == 1 ? true : false
  },

  methods: {
    saveChanges() {
      if (this.loading) return;
      this.loading = true;

      setTimeout(() => {
        axios
          .put(`/user-settings/my-info`, this.formData)
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
            this.errors = error.response?.data?.errors;
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