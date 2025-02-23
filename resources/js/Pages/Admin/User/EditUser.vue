<template>
  <app-layout>
    <app-head :title="__('Edit User @:username', {username: userData.username})" />

    <div class="py-12 px-10 max-w-6xl mx-auto">
      <div class="flex justify-between mb-8">
        <h1 class="font-bold text-3xl text-gray-500 dark:text-gray-300">
          {{ __("Edit User ':username'", {username: userData.name}) }}
        </h1>
        <inertia-link
          :href="route('admin.user.index')"
          class="inline-flex items-center px-4 py-2 bg-gray-400 dark:bg-gray-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-500 active:bg-gray-600 focus:outline-none focus:border-gray-500 focus:shadow-outline-gray transition ease-in-out duration-150"
        >
          <span>{{ __("Cancel") }}</span>
        </inertia-link>
      </div>


      <div class="mt-10 sm:mt-0">
        <div class="md:grid md:grid-cols-3 md:gap-6">
          <div class="md:col-span-1">
            <div class="px-4 sm:px-0">
              <h3 class="text-lg font-medium leading-6 text-gray-900 dark:text-gray-400">
                {{ __("Tips!") }}
              </h3>
              <p class="mt-1 text-sm text-gray-600 dark:text-gray-500">
                {{ __("You can change role of a user in this section") }}
              </p>
            </div>
          </div>

          <div class="mt-5 md:mt-0 md:col-span-2">
            <form @submit.prevent="updateUserInformation">
              <div class="shadow sm:rounded-md">
                <div class="px-4 py-5 bg-white dark:bg-cool-gray-800 sm:p-6">
                  <div class="grid grid-cols-6 gap-6">
                    <div
                      v-if="$page.props.jetstream.managesProfilePhotos"
                      class="hidden col-span-6 sm:col-span-4"
                    >
                      <!-- Profile Photo File Input -->
                      <input
                        ref="photo"
                        type="file"
                        class="hidden"
                        @change="updatePhotoPreview"
                      >

                      <jet-label
                        for="photo"
                        value="Photo"
                      />

                      <!-- Current Profile Photo -->
                      <div
                        v-show="! photoPreview"
                        class="mt-2"
                      >
                        <img
                          :src="userData.profile_photo_url"
                          :alt="userData.name"
                          class="rounded-full h-20 w-20 object-cover"
                        >
                      </div>

                      <!-- New Profile Photo Preview -->
                      <div
                        v-show="photoPreview"
                        class="mt-2"
                      >
                        <span
                          class="block rounded-full w-20 h-20"
                          :style="'background-size: cover; background-repeat: no-repeat; background-position: center center; background-image: url(\'' + photoPreview + '\');'"
                        />
                      </div>

                      <jet-secondary-button
                        class="mt-2 mr-2"
                        type="button"
                        @click.native.prevent="selectNewPhoto"
                      >
                        {{ __("Select A New Photo") }}
                      </jet-secondary-button>

                      <jet-input-error
                        :message="form.errors.photo"
                        class="mt-2"
                      />
                    </div>

                    <!-- Username -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="username"
                        v-model="form.username"
                        :label="__('Username')"
                        :error="form.errors.username"
                        type="text"
                        name="username"
                      />
                    </div>

                    <!-- Email -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="email"
                        v-model="form.email"
                        :label="__('Email Address')"
                        :error="form.errors.email"
                        type="email"
                        name="email"
                      />
                    </div>

                    <!-- Name -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="name"
                        v-model="form.name"
                        :label="__('Full Name')"
                        :error="form.errors.name"
                        type="text"
                        name="name"
                      />
                    </div>

                    <div class="col-span-6 sm:col-span-3">
                      <x-select
                        id="profile_photo_source"
                        v-model="form.profile_photo_source"
                        name="profile_photo_source"
                        :error="form.errors.profile_photo_source"
                        :label="__('Use Avatar from')"
                        :select-list="{linked_player: __('Linked Player'), gravatar: __('Gravatar')}"
                        :placeholder="__('Uploaded Photo')"
                      />
                    </div>

                    <!-- DOB -->
                    <div class="col-span-6 sm:col-span-3 relative">
                      <date-picker
                        id="dob"
                        v-model="form.dob"
                        :placeholder="__('Select your date of birth')"
                        class="w-full"
                        value-type="format"
                        input-class="border-gray-300 h-14 p-3 text-sm pt-7 focus:border-light-blue-300 focus:ring focus:ring-light-blue-200 focus:ring-opacity-50 rounded-md block w-full dark:bg-cool-gray-900 dark:text-gray-300 dark:border-gray-900"
                      />

                      <label
                        for="dob"
                        class="absolute -top-2.5 left-0 px-3 py-5 text-xs text-gray-500 h-full pointer-events-none transform origin-left transition-all duration-100 ease-in-out "
                      >{{ __("Date of Birth") }}</label>
                      <jet-input-error
                        :message="form.errors.dob"
                        class="mt-1"
                      />
                    </div>

                    <div
                      v-if="form.dob"
                      class="col-span-6 sm:col-span-3"
                    >
                      <x-checkbox
                        id="show_yob"
                        v-model="form.show_yob"
                        :label="__('Show Your of Birth')"
                        :help="__('Show Year of Birth in your public profile.')"
                        name="show_yob"
                        :error="form.errors.show_yob"
                      />
                    </div>

                    <!-- Gender -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-select
                        id="gender"
                        v-model="form.gender"
                        name="gender"
                        :error="form.errors.gender"
                        :label="__('Gender')"
                        placeholder="Prefer not to say"
                        :select-list="{m: __('Male'), f: __('Female'), o: __('Others')}"
                      />
                    </div>

                    <div
                      v-if="form.gender"
                      class="col-span-6 sm:col-span-3"
                    >
                      <x-checkbox
                        id="show_gender"
                        v-model="form.show_gender"
                        :label="__('Show Gender')"
                        :help="__('Show Gender in your public profile.')"
                        name="show_gender"
                        :error="form.errors.show_gender"
                      />
                    </div>

                    <!-- s_discord_username -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_discord_username"
                        v-model="form.s_discord_username"
                        :label="__('Discord Username')"
                        :error="form.errors.s_discord_username"
                        autocomplete="s_discord_username"
                        type="text"
                        name="s_discord_username"
                        :help="__('Eg: username#1234')"
                      />
                    </div>

                    <!-- s_steam_profile_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_steam_profile_url"
                        v-model="form.s_steam_profile_url"
                        :label="__('Steam Profile URL')"
                        :error="form.errors.s_steam_profile_url"
                        autocomplete="s_steam_profile_url"
                        type="text"
                        name="s_steam_profile_url"
                        :help="__('Eg: https://steamcommunity.com/id/username')"
                      />
                    </div>

                    <!-- s_twitter_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_twitter_url"
                        v-model="form.s_twitter_url"
                        :label="__('Twitter Profile URL')"
                        :error="form.errors.s_twitter_url"
                        autocomplete="s_twitter_url"
                        type="text"
                        name="s_twitter_url"
                        :help="__('Eg: https://twitter.com/@username')"
                      />
                    </div>

                    <!-- s_youtube_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_youtube_url"
                        v-model="form.s_youtube_url"
                        :label="__('YouTube URL')"
                        :error="form.errors.s_youtube_url"
                        autocomplete="s_youtube_url"
                        type="text"
                        name="s_youtube_url"
                        :help="__('Eg: https://www.youtube.com/minecraft')"
                      />
                    </div>

                    <!-- s_facebook_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_facebook_url"
                        v-model="form.s_facebook_url"
                        :label="__('Facebook URL')"
                        :error="form.errors.s_facebook_url"
                        autocomplete="s_facebook_url"
                        type="text"
                        name="s_facebook_url"
                        :help="__('Eg: https://www.facebook.com/minecraft')"
                      />
                    </div>

                    <!-- s_twitch_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_twitch_url"
                        v-model="form.s_twitch_url"
                        :label="__('Twitch URL')"
                        :error="form.errors.s_twitch_url"
                        autocomplete="s_twitch_url"
                        type="text"
                        name="s_twitch_url"
                        :help="__('Eg: https://www.twitch.tv/minecraft')"
                      />
                    </div>

                    <!-- s_website_url -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-input
                        id="s_website_url"
                        v-model="form.s_website_url"
                        :label="__('Website URL')"
                        :error="form.errors.s_website_url"
                        autocomplete="s_website_url"
                        type="text"
                        name="s_website_url"
                        :help="__('Eg: https://my-personal-blog.com')"
                      />
                    </div>

                    <!-- Role  -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-select
                        id="role"
                        v-model="form.role"
                        name="role"
                        :error="form.errors.role"
                        label="Role"
                        :placeholder="__('Select role')"
                        :select-list="rolesList"
                      />
                      <!--                                        <jet-label for="role" value="Role"/>-->
                      <!--                                        <multiselect v-model="form.role" deselect-label="Can't remove" track-by="id" label="display_name" placeholder="Select role" :options="rolesList" :searchable="false" :allow-empty="false"></multiselect>-->
                      <!--                                        <jet-input-error :message="form.errors.role" class="mt-2"/>-->
                    </div>

                    <!-- about -->
                    <div class="col-span-6 sm:col-span-3">
                      <x-textarea
                        id="about"
                        v-model="form.about"
                        :rows="10"
                        :label="__('About Yourself')"
                        :help="__('Something about yourself in 255 characters.')"
                        :error="form.errors.about"
                        name="about"
                      />
                    </div>

                    <div class="col-span-6 sm:col-span-3">
                      <x-checkbox
                        id="verified"
                        v-model="form.verified"
                        :label="__('Verified User')"
                        :help="__('Show a blue verified tick after username.')"
                        name="verified"
                        :error="form.errors.verified"
                      />
                    </div>
                  </div>
                </div>
                <div class="px-4 py-3 bg-gray-50 dark:bg-cool-gray-800 sm:px-6 flex justify-end">
                  <loading-button
                    :loading="form.processing"
                    class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-light-blue-600 hover:bg-light-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-light-blue-500 disabled:opacity-50"
                    type="submit"
                  >
                    {{ __("Update User") }}
                  </loading-button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </app-layout>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout';
import JetInputError from '@/Jetstream/InputError';
import JetSecondaryButton from '@/Jetstream/SecondaryButton';
import LoadingButton from '@/Components/LoadingButton';
import XInput from '@/Components/Form/XInput';
import JetLabel from '@/Jetstream/Label';
import DatePicker from 'vue2-datepicker';
import XCheckbox from '@/Components/Form/XCheckbox';
import XSelect from '@/Components/Form/XSelect';
import XTextarea from '@/Components/Form/XTextarea';

export default {

    components: {
        XTextarea,
        XSelect,
        XCheckbox,
        AppLayout,
        JetInputError,
        LoadingButton,
        JetSecondaryButton,
        XInput,
        JetLabel,
        DatePicker
    },
    props: {
        userData: Object,
        rolesList: Object
    },
    data() {
        return {
            form: this.$inertia.form({
                _method: 'PUT',
                username: this.userData.username,
                name: this.userData.name,
                email: this.userData.email,
                photo: null,
                dob: this.userData.dob,
                gender: this.userData.gender,
                cover_image_url: this.userData.cover_image_url,
                s_discord_username: this.userData.social_links ? this.userData.social_links.s_discord_username : null,
                s_steam_profile_url: this.userData.social_links ? this.userData.social_links.s_steam_profile_url : null,
                s_twitter_url: this.userData.social_links ? this.userData.social_links.s_twitter_url : null,
                s_youtube_url: this.userData.social_links ? this.userData.social_links.s_youtube_url : null,
                s_facebook_url: this.userData.social_links ? this.userData.social_links.s_facebook_url : null,
                s_twitch_url: this.userData.social_links ? this.userData.social_links.s_twitch_url : null,
                s_website_url: this.userData.social_links ? this.userData.social_links.s_website_url : null,
                about: this.userData.about,
                profile_photo_source: this.userData.settings ? this.userData.settings.profile_photo_source : null,
                show_gender: this.userData.settings ? this.userData.settings.show_gender : false,
                show_yob: this.userData.settings ? this.userData.settings.show_yob : false,
                verified: !!this.userData.verified_at,
                role: this.userData.roles[0].name
            }),

            photoPreview: null,
        };
    },

    methods: {
        updateUserInformation() {
            if (this.$refs.photo) {
                this.form.photo = this.$refs.photo.files[0];
            }

            // const tempRole = this.form.role
            // this.form.role = this.form.role.name
            this.form.post(route('admin.user.update', this.userData.id), {
                preserveScroll: true
            });
            // this.form.role = tempRole
        },

        selectNewPhoto() {
            this.$refs.photo.click();
        },

        updatePhotoPreview() {
            const reader = new FileReader();

            reader.onload = (e) => {
                this.photoPreview = e.target.result;
            };

            reader.readAsDataURL(this.$refs.photo.files[0]);
        },
    },
};
</script>


<style scoped>
.mx-datepicker {
    width: 100% !important;
}
</style>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
