<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleLogin">
      <p class="col-12 text-h5 text-center">Login</p>
      <div class="cold-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input
          label="Email"
          v-model="form.email"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Email is required!']"
        ></q-input>
        <q-input
          label="Password"
          v-model="form.password"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Password is required!']"
        ></q-input>
        <div class="full-width q-pt-md">
          <q-btn
            label="Login"
            color="primary"
            class="full-width"
            outline
            rounded
            type="submit"
          ></q-btn>
        </div>
        <div class="full-width">
          <q-btn
            label="Register"
            color="primary"
            flat
            to="/register"
            size="sm"
            class="full-width"
          ></q-btn>
          <q-btn
            label="Forgot Password"
            color="primary"
            flat
            to="/forgot-password"
            size="sm"
            class="full-width"
          ></q-btn>
        </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import useAuthUser from "src/composables/UseAuthUser";
import { useRouter } from "vue-router";
import useNotify from "src/composables/UseNotify";

export default defineComponent({
  name: "PageLogin",
  setup() {
    const router = useRouter();
    const form = ref({ email: "", password: "" });
    const { login, isLoggedIn } = useAuthUser();
    const { notifyError } = useNotify();

    const handleLogin = async () => {
      try {
        await login(form.value);
        router.push({ name: "me" });
      } catch (error) {
        notifyError(error.message);
      }
    };

    onMounted(() => {
      if (isLoggedIn()) router.push({ name: "me" });
    });

    return {
      form,
      handleLogin,
    };
  },
});
</script>
