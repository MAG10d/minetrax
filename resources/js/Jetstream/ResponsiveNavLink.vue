<template>
  <div>
    <button
      v-if="as == 'button'"
      :class="classes"
      class="w-full text-left"
    >
      <slot />
    </button>

    <inertia-link
      v-else
      :href="href"
      :class="classes"
    >
      <slot />
    </inertia-link>
  </div>
</template>

<script>
export default {
    props: ['active', 'href', 'as'],

    computed: {
        classes() {
            return this.active
                ? 'block pl-3 pr-4 py-2 border-l-4 border-light-blue-400 text-base font-medium text-light-blue-700 bg-light-blue-50 dark:bg-cool-gray-900 focus:outline-none focus:text-light-blue-800 focus:bg-light-blue-100 dark:focus:bg-cool-gray-900 focus:border-light-blue-700 transition duration-150 ease-in-out'
                : 'block pl-3 pr-4 py-2 border-l-4 border-transparent text-base font-medium text-gray-600 dark:text-gray-400 hover:text-gray-800 dark:hover:text-gray-200 hover:bg-gray-50 dark:hover:bg-cool-gray-700 hover:border-gray-300 focus:outline-none focus:text-gray-800 dark:focus:text-gray-200 focus:bg-gray-50 dark:focus:bg-cool-gray-900 focus:border-gray-300 transition duration-150 ease-in-out';
        }
    }
};
</script>
