<template>
  <q-page padding class="text-center">
    <p class="text-body1">Thans for registering!</p>
    <p class="text-body1">
      Please confirm your email to finishing registering:
      <strong>{{ $route.query.email }}</strong>
    </p>
    <q-btn
      label="Login"
      color="primary"
      outline
      rounded
      @click="$router.push({ name: 'login' })"
    ></q-btn>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageEmailConfirmation",
});
</script>
