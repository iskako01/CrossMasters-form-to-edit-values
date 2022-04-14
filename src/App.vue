<template>
  <AppForm />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AppForm from "@/components/AppForm.vue";

export default defineComponent({
  name: "App",
  components: { AppForm },
});
</script>
