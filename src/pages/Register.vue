<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleRegister">
      <p class="col-12 text-h5 text-center">Register</p>
      <div class="cold-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input
          label="Name"
          v-model="form.name"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Name is required!']"
        ></q-input>
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
            label="Register"
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
import { defineComponent, ref } from "vue";
import useAuthUser from "src/composables/UseAuthUser";
import { useRouter } from "vue-router";
import useNotify from "src/composables/UseNotify";

export default defineComponent({
  name: "PageRegister",
  setup() {
    const router = useRouter();
    const { register } = useAuthUser();
    const form = ref({ email: "", password: "", name: "" });
    const { notifyError } = useNotify();

    const handleRegister = async () => {
      try {
        await register(form.value);
        router.push({
          name: "email-confirmation",
          query: { email: form.value.email },
        });
      } catch (error) {
        notifyError(error.message);
      }
    };

    return {
      form,
      handleRegister,
    };
  },
});
</script>
