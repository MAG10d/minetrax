<template>
  <div class="md:col-span-1">
    <div class="px-4 sm:px-0">
      <h3 class="text-lg font-medium text-gray-900 dark:text-gray-200">
        <slot name="title" />
      </h3>

      <p class="mt-1 text-sm text-gray-600 dark:text-gray-500">
        <slot name="description" />
      </p>
    </div>
  </div>
</template>
