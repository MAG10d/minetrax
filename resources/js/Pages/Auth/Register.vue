<template>
  <app-layout>
    <app-head
      :title="__('Register')"
    />

    <jet-authentication-card>
      <template #logo>
        <jet-authentication-card-logo />
      </template>

      <!--<jet-validation-errors class="mb-4" />-->

      <form
        class="mt-5"
        @submit.prevent="submit"
      >
        <div>
          <x-input
            id="name"
            v-model="form.name"
            :label="__('Full Name')"
            :error="form.errors.name"
            autocomplete="name"
            :autofocus="true"
            :required="true"
            type="text"
            name="name"
          />
        </div>

        <div class="mt-4">
          <x-input
            id="email"
            v-model="form.email"
            :label="__('Email Address')"
            :error="form.errors.email"
            :required="true"
            type="email"
            name="email"
          />
        </div>

        <div class="mt-4">
          <x-input
            id="username"
            v-model="form.username"
            :label="__('Username')"
            :error="form.errors.username"
            :required="true"
            type="text"
            name="username"
          />
        </div>

        <div class="mt-4">
          <x-input
            id="password"
            v-model="form.password"
            :label="__('Password')"
            :error="form.errors.password"
            :required="true"
            autocomplete="new-password"
            type="password"
            name="password"
          />
        </div>

        <div class="mt-4">
          <x-input
            id="password_confirmation"
            v-model="form.password_confirmation"
            :label="__('Confirm Password')"
            :error="form.errors.password_confirmation"
            :required="true"
            autocomplete="new-password"
            type="password"
            name="password_confirmation"
          />
        </div>

        <password
          v-model="form.password"
          :strength-meter-only="true"
          strength-meter-class="Password__strength-meter dark:bg-cool-gray-700"
        />

        <div
          v-if="$page.props.jetstream.hasTermsAndPrivacyPolicyFeature"
          class="mt-4"
        >
          <jet-label for="terms">
            <div class="flex items-center">
              <jet-checkbox
                id="terms"
                v-model="form.terms"
                name="terms"
              />

              <div class="ml-2">
                {{ __("I agree to the ") }} <a
                  target="_blank"
                  :href="route('terms.show')"
                  class="underline text-sm text-gray-600 hover:text-gray-900"
                >{{ __("Terms of Service") }}</a>&nbsp;{{ __("and") }}&nbsp;<a
                  target="_blank"
                  :href="route('policy.show')"
                  class="underline text-sm text-gray-600 hover:text-gray-900"
                >{{ __("Privacy Policy") }}</a>
              </div>
            </div>
          </jet-label>
        </div>

        <div class="flex items-center justify-end mt-4">
          <inertia-link
            :href="route('login')"
            class="underline text-sm text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-200"
          >
            {{ __("Already registered?") }}
          </inertia-link>

          <loading-button
            :loading="form.processing"
            class="ml-4 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-light-blue-500 hover:bg-light-blue-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-light-blue-500 disabled:opacity-50"
          >
            {{ __("Register") }}
          </loading-button>
        </div>

        <social-auth-buttons />
      </form>
    </jet-authentication-card>
  </app-layout>
</template>

<script>
import Password from 'vue-password-strength-meter';
import JetAuthenticationCard from '@/Jetstream/AuthenticationCard';
import JetAuthenticationCardLogo from '@/Jetstream/AuthenticationCardLogo';
import JetCheckbox from '@/Jetstream/Checkbox';
import JetLabel from '@/Jetstream/Label';
import LoadingButton from '@/Components/LoadingButton';
import AppLayout from '@/Layouts/AppLayout';
import SocialAuthButtons from '@/Components/SocialAuthButtons';
import XInput from '@/Components/Form/XInput';

export default {
    components: {
        XInput,
        SocialAuthButtons,
        AppLayout,
        LoadingButton,
        JetAuthenticationCard,
        JetAuthenticationCardLogo,
        JetCheckbox,
        JetLabel,
        Password
    },

    data() {
        return {
            form: this.$inertia.form({
                name: '',
                email: '',
                username: '',
                password: '',
                password_confirmation: '',
                terms: false,
            })
        };
    },

    methods: {
        submit() {
            this.form.post(this.route('register'), {
                onFinish: () => this.form.reset('password', 'password_confirmation'),
            });
        }
    }
};
</script>
