<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleForgotPassword">
      <p class="col-12 text-h5 text-center">Forgot Password</p>
      <div class="cold-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input
          label="Email"
          v-model="email"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Email is required!']"
        ></q-input>
        <div class="full-width q-pt-md">
          <q-btn
            label="Send Reset Email"
            color="primary"
            class="full-width"
            outline
            rounded
            type="submit"
          ></q-btn>
        </div>
        <div class="full-width">
          <q-btn
            label="Back"
            color="primary"
            class="full-width"
            flat
            to="/login"
          ></q-btn>
        </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import useAuthUser from "src/composables/UseAuthUser";
import { defineComponent, ref } from "vue";
import useNotify from "src/composables/UseNotify";

export default defineComponent({
  name: "PageForgotPassword",
  setup() {
    const { sendPasswordRestEmail } = useAuthUser();
    const email = ref("");
    const { notifySuccess, notifyError } = useNotify();
    const handleForgotPassword = async () => {
      try {
        await sendPasswordRestEmail(email.value);
        notifySuccess(`Password reset email sent to: ${email.value}`);
      } catch (error) {
        notifyError(error.message);
      }
    };

    return {
      email,
      handleForgotPassword,
    };
  },
});
</script>
