<template>
  <div class="md:grid md:grid-cols-3 md:gap-6">
    <jet-section-title>
      <template #title>
        <slot name="title" />
      </template>
      <template #description>
        <slot name="description" />
      </template>
    </jet-section-title>

    <div class="mt-5 md:mt-0 md:col-span-2">
      <div class="px-4 py-5 sm:p-6 bg-white dark:bg-cool-gray-800 shadow sm:rounded-lg">
        <slot name="content" />
      </div>
    </div>
  </div>
</template>

<script>
import JetSectionTitle from './SectionTitle';

export default {
    components: {
        JetSectionTitle,
    }
};
</script>
