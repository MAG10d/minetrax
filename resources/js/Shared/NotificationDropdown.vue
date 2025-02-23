<template>
  <jet-dropdown
    width="full"
    class="ml-4"
  >
    <template #trigger>
      <icon
        name="bell"
        class="w-5 h-5 text-gray-700 dark:text-gray-200"
      />
    </template>

    <template #content>
      <div class="container p-3 pb-2 w-80">
        <div class="flex justify-between">
          <div class="block text-xs text-gray-400">
            {{ __("Notifications") }}
          </div>
          <inertia-link
            as="button"
            :href="route('notification.mark-as-read')"
            method="post"
            :preserve-state="false"
            class="block text-xs text-light-blue-400 hover:underline"
          >
            {{ __("Mark all as read") }}
          </inertia-link>
        </div>

        <!-- Loading Spinner -->
        <div
          v-if="loading"
          class="flex p-4 justify-center"
        >
          <svg
            class="animate-spin -ml-1 mr-3 h-5 w-5 text-light-blue-600"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            />
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            />
          </svg>
        </div>

        <error-message v-if="error">
          {{ error }}
        </error-message>

        <div class="flex flex-col text-sm text-gray-700 dark:text-gray-300 mt-2 h-96 overflow-y-auto space-y-1">
          <notification
            v-for="notification in notifications.data"
            :key="notification.id"
            :notification="notification"
          />
          <div
            v-if="!loading && notifications.data.length <= 0"
            :key="999999999"
            class="flex items-center justify-center italic text-gray-500 dark:text-gray-400 p-4"
          >
            {{ __("No notifications to show.") }}
          </div>
        </div>

        <div class="flex justify-center text-light-blue-400 text-xs mt-2 hover:underline cursor-pointer">
          <inertia-link
            as="a"
            :href="route('notification.index')"
          >
            {{ __("View All") }}
          </inertia-link>
        </div>
      </div>
    </template>
  </jet-dropdown>
</template>

<script>
import JetDropdown from '@/Jetstream/Dropdown';
import Icon from '@/Components/Icon';
import Notification from '@/Components/Notification';
import ErrorMessage from '@/Components/ErrorMessage';

export default {
    name: 'NotificationDropdown',
    components: {
        ErrorMessage,
        Notification,
        Icon,
        JetDropdown,
    },
    data() {
        return {
            loading: true,
            error: null,
            notifications: {}
        };
    },
    mounted() {
        let routeToHit = route('notification.index');
        axios.get(routeToHit).then(response => {
            this.notifications = response.data;
        }).finally(() => {
            this.loading = false;
        });
    }
};
</script>
