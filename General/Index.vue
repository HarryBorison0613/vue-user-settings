<script setup>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue';
import { Head, Link } from '@inertiajs/inertia-vue3';
import { ref } from "vue";
import { Inertia } from "@inertiajs/inertia";
import MyInformation from "@/Components/UserSettings/MyInformation.vue";
import NotificationAlerts from "@/Components/UserSettings/NotificationAlerts.vue";
import ChangePassword from "@/Components/UserSettings/ChangePassword.vue";
import ProfilePicture from "@/Components/UserSettings/ProfilePicture.vue";

const props = defineProps({
  userInfo: Object,
  userSettings: Object,
  states: Array,
  timezones: Array,
})

const profile_picture = ref(props.userInfo.profile_picture);
const loading = ref(false);
const currentTab = ref("my-information");
const tabs = [
  { key: 'my-information', label: 'My Information' },
  { key: 'notification-alerts', label: 'Notification & Alerts' },
  { key: 'change-password', label: 'Change Password' },
  { key: 'profile-picture', label: 'Profile Picture' },
]

const setCurrentTab = (tabKey) => {
  currentTab.value = tabKey;
}

const setProfilePicuture = (path) => {
  console.log('setProfilePicuture', path)
  profile_picture.value = path;
}

const syncData = () => {
  console.log('syncData')
  Inertia.get(route('user-settings-general.index'), {}, {
    preserveState: true,
    preserveScroll: true,
    replace: true,
    // only: ['userInfo', 'userSettings']
  })
}


</script>

<template>

  <Head title="General Settings" />

  <BreezeAuthenticatedLayout>
    <div class="px-4 sm:px-6 lg:px-8">
      <div class="col-span-12 flex items-center justify-between">
        <div class="lg:hidden w-full">
          <label for="tabs" class="sr-only">Select a tab</label>
          <!-- Use an "onChange" listener to redirect the user to the selected tab URL. -->
          <select @change="setCurrentTab($event.target.value)" id="tabs" name="tabs"
            class="block w-full rounded-md border-gray-300 focus:border-tb-blue focus:ring-tb-blue">
            <option v-for="tab in tabs" :key="tab.key" :value="tab.key" :selected="currentTab === tab.key">{{
              tab.label
            }}
            </option>
          </select>
        </div>
        <div class="hidden lg:block w-full">
          <nav class="flex space-x-1 shadow ring-1 ring-black ring-opacity-5 md:rounded-lg p-2 bg-white"
            aria-label="Tabs">
            <a @click.prevent="setCurrentTab(tab.key)" v-for="tab in tabs" :key="tab.key" href="javascript:;"
              :class="[currentTab === tab.key ? 'bg-tb-blue text-white shadow' : 'text-gray-500 hover:text-gray-700', 'px-6 py-2 font-semibold rounded-md']"
              :aria-current="currentTab === tab.key ? 'page' : undefined">{{ tab.label }}</a>
          </nav>
        </div>
      </div>

      <div class="col-span-12 xl:col-span-8 mt-6">
        <div class="grid grid-cols-12 gap-6">
          <div class="col-span-12 rounded-lg">
            <div class="tab-content min-h-[300px]">
              <div v-if="currentTab === tabs[0].key" :id="tabs[0].key"
                class="tab-pane leading-relaxed p-5 bg-white rounded-lg border" role="tabpanel"
                :aria-labelledby="tabs[0].key">
                <MyInformation @sync-data="syncData" :userInfo="userInfo" :userSettings="userSettings" :states="states"
                  :timezones="timezones" />
              </div>

              <div v-else-if="currentTab === tabs[1].key" :id="tabs[1].key"
                class="tab-pane leading-relaxed p-5 pt-8 bg-white rounded-lg border" role="tabpanel"
                :aria-labelledby="tabs[1].key">
                <NotificationAlerts @sync-data="syncData" :userSettings="userSettings" />
              </div>

              <div v-else-if="currentTab === tabs[2].key" :id="tabs[2].key"
                class="tab-pane leading-relaxed p-5 bg-white rounded-lg border" role="tabpanel"
                :aria-labelledby="tabs[2].key">
                <ChangePassword />
              </div>

              <div v-else-if="currentTab === tabs[3].key" :id="tabs[3].key"
                class="tab-pane leading-relaxed p-2 md:p-5 bg-white rounded-lg border" role="tabpanel"
                :aria-labelledby="tabs[3].key">
                <ProfilePicture :profile_picture="profile_picture" @set-profile-picuture="setProfilePicuture" />
              </div>

            </div>
          </div>
        </div>
      </div>

    </div>
  </BreezeAuthenticatedLayout>
</template>