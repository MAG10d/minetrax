<template>
  <app-layout>
    <app-head
      :title="__('Features & Todo List')"
    />

    <div class="py-3 px-2 sm:py-12 sm:px-10 max-w-7xl mx-auto">
      <h1 class="text-center text-xl sm:text-2xl md:text-4xl font-extrabold text-gray-700 dark:text-gray-200 mb-3">
        {{ __("Features & Known Bugs") }}
      </h1>

      <span class="text-lg font-extrabold text-green-500 mb-4 flex items-center">
        <icon
          name="verified-check-fill"
          class="h-8 w-8 animate-bounce"
        /> {{ __("Completed") }}
      </span>
      <div class="grid sm:grid-cols-2 md:grid-cols-4 gap-4">
        <feature-card
          v-for="(feature, index) in features"
          v-if="feature.status === 'completed'"
          :key="feature.todoId"
          :feature="feature"
          :index="index+2"
        />
      </div>

      <span class="text-lg mt-10 font-extrabold text-light-blue-500 mb-4 flex items-center">
        <icon
          name="spin-loader"
          class="h-7 w-7 animate-spin mr-1"
        /> {{ __("In Progress") }}
      </span>
      <div class="grid sm:grid-cols-2 md:grid-cols-4 gap-4">
        <feature-card
          v-for="(feature, index) in features"
          v-if="feature.status === 'in-progress'"
          :key="feature.todoId"
          :feature="feature"
          :index="index+4"
        />
      </div>

      <span class="text-lg mt-10 font-extrabold text-pink-500 mb-4 flex items-center">
        <icon
          class=" w-7 h-7 animate-ping absolute inline-flex opacity-75"
          name="heart-fill"
        />
        <icon
          class="w-7 h-7 relative inline-flex mr-1"
          name="heart-fill"
        />
        {{ __("Draft") }}
      </span>
      <div class="grid sm:grid-cols-2 md:grid-cols-4 gap-4">
        <feature-card
          v-for="(feature, index) in features"
          v-if="feature.status === 'draft'"
          :key="feature.todoId"
          :feature="feature"
          :index="index+2"
        />
      </div>
    </div>

    <span class="absolute hidden bg-red-100 bg-light-blue-100 bg-green-100 bg-pink-100 bg-orange-100 bg-purple-100 border-red-500 border-light-blue-500 border-green-500 border-pink-500 border-orange-500 border-purple-500 text-red-500 text-light-blue-500 text-green-500 text-pink-500 text-orange-500 text-purple-500" />
  </app-layout>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout';
import Icon from '@/Components/Icon';
import FeatureCard from '@/Components/FeatureCard';

export default {

    components: {
        FeatureCard,
        Icon,
        AppLayout,
    },
    props: {
        features: Array
    },
};
</script>
