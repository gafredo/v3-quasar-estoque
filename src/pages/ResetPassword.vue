<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handlePasswordReset">
      <p class="col-12 text-h5 text-center">Reset Password</p>
      <div class="cold-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input
          label="New Password"
          v-model="newPassword"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Password is required!']"
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
      </div>
    </q-form>
  </q-page>
</template>

<script>
import useAuthUser from "src/composables/UseAuthUser";
import { defineComponent, ref } from "vue";
import { useRouter, useRoute } from "vue-router";
import useNotify from "src/composables/UseNotify";

export default defineComponent({
  name: "PageResetPassword",
  setup() {
    const { resetPassword } = useAuthUser();
    const newPassword = ref("");
    const router = useRouter();
    const route = useRoute();
    const token = route.query.token;
    const { notifyError } = useNotify();

    const handlePasswordReset = async () => {
      try {
        await resetPassword(token, newPassword.value);
        router.push({ name: "login" });
      } catch (error) {
        notifyError(error.message);
      }
    };

    return {
      newPassword,
      handlePasswordReset,
    };
  },
});
</script>
