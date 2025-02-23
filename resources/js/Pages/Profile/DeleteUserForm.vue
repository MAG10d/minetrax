<template>
  <jet-action-section>
    <template #title>
      {{ __("Delete Account") }}
    </template>

    <template #description>
      {{ __("Permanently delete your account.") }}
    </template>

    <template #content>
      <div class="max-w-xl text-sm text-gray-600">
        {{ __("Once your account is deleted, all of its resources and data will be permanently deleted. Before deleting your account, please download any data or information that you wish to retain.") }}
      </div>

      <div class="mt-5">
        <jet-danger-button @click.native="confirmUserDeletion">
          {{ __("Delete Account") }}
        </jet-danger-button>
      </div>

      <!-- Delete Account Confirmation Modal -->
      <jet-dialog-modal
        :show="confirmingUserDeletion"
        @close="closeModal"
      >
        <template #title>
          {{ __("Delete Account") }}
        </template>

        <template #content>
          {{ __("Are you sure you want to delete your account? Once your account is deleted, all of its resources and data will be permanently deleted. Please enter your password to confirm you would like to permanently delete your account.") }}

          <div class="mt-4">
            <jet-input
              ref="password"
              v-model="form.password"
              type="password"
              class="mt-1 block w-3/4"
              placeholder="Password"
              @keyup.enter.native="deleteUser"
            />

            <jet-input-error
              :message="form.errors.password"
              class="mt-2"
            />
          </div>
        </template>

        <template #footer>
          <jet-secondary-button @click.native="closeModal">
            {{ __("Nevermind") }}
          </jet-secondary-button>

          <jet-danger-button
            class="ml-2"
            :class="{ 'opacity-25': form.processing }"
            :disabled="form.processing"
            @click.native="deleteUser"
          >
            {{ __("Delete Account") }}
          </jet-danger-button>
        </template>
      </jet-dialog-modal>
    </template>
  </jet-action-section>
</template>

<script>
import JetActionSection from '@/Jetstream/ActionSection';
import JetDialogModal from '@/Jetstream/DialogModal';
import JetDangerButton from '@/Jetstream/DangerButton';
import JetInput from '@/Jetstream/Input';
import JetInputError from '@/Jetstream/InputError';
import JetSecondaryButton from '@/Jetstream/SecondaryButton';

export default {
    components: {
        JetActionSection,
        JetDangerButton,
        JetDialogModal,
        JetInput,
        JetInputError,
        JetSecondaryButton,
    },

    data() {
        return {
            confirmingUserDeletion: false,

            form: this.$inertia.form({
                password: '',
            })
        };
    },

    methods: {
        confirmUserDeletion() {
            this.confirmingUserDeletion = true;

            setTimeout(() => this.$refs.password.focus(), 250);
        },

        deleteUser() {
            this.form.delete(route('current-user.destroy'), {
                preserveScroll: true,
                onSuccess: () => this.closeModal(),
                onError: () => this.$refs.password.focus(),
                onFinish: () => this.form.reset(),
            });
        },

        closeModal() {
            this.confirmingUserDeletion = false;

            this.form.reset();
        },
    },
};
</script>
