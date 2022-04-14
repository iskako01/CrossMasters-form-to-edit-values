<template>
  <div class="container">
    <form @submit.prevent class="card">
      <div class="form-control">
        <label for="primaryColor">primaryColor</label>
        <input type="text" id="primaryColor" v-model="options.primaryColor" />
      </div>

      <div class="form-control">
        <label for="borderRadius">borderRadius</label>
        <input type="number" id="borderRadius" v-model="options.borderRadius" />
      </div>

      <div class="form-control">
        <label for="dismissType">dismissType</label>
        <select id="dismissType" v-model="options.dismissType">
          <option value="cross">cross</option>
          <option value="cross-faint">cross-faint</option>
          <option value="text">text</option>
        </select>
      </div>

      <div class="form-control">
        <label for="expiration">expiration</label>
        <input type="number" id="expiration" v-model="options.expiration" />
      </div>

      <div class="form-checkbox">
        <label for="dismissable">dismissable</label>
        <input type="checkbox" id="dismissable" v-model="options.dismissable" />
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import iOptions from "@/types/options";

export default defineComponent({
  name: "AppForm",
  setup() {
    const options = ref<iOptions>({
      dataLayer: "dataLayer",
      primaryColor: "#f98305",
      borderRadius: 6,
      dismissable: false,
      dismissType: "text",
      expiration: 365,
      closeType: "cross",
    });

    return { options };
  },
});
</script>
