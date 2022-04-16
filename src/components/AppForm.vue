<template>
  <div class="container">
    <form @submit.prevent="onSubmit" class="card">
      <div class="form-control">
        <label for="primaryColor">primaryColor</label>
        <input
          type="text"
          id="primaryColor"
          v-model="primaryColor"
          @blur="pBlur"
        />
        <small v-if="pError">{{ pError }}</small>
      </div>

      <div class="form-control">
        <label for="borderRadius">borderRadius</label>
        <input
          type="number"
          id="borderRadius"
          v-model="borderRadius"
          @blur="bBlur"
        />
        <small v-if="bError">{{ bError }}</small>
      </div>

      <div class="form-control">
        <label for="dismissType">dismissType</label>
        <select id="dismissType" v-model="dismissType">
          <option value="cross">cross</option>
          <option value="cross-faint">cross-faint</option>
          <option value="text">text</option>
        </select>
        <small v-if="dtError">{{ dtError }}</small>
      </div>

      <div class="form-control">
        <label for="expiration">expiration</label>
        <input type="number" id="expiration" v-model="expiration" />
        <small v-if="eError">{{ eError }}</small>
      </div>

      <div class="form-checkbox">
        <input type="checkbox" class="checkbox" v-model="dismissable" />
        <small v-if="dError">{{ dError }}</small>
      </div>

      <button class="btn">Send</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import * as yup from "yup";
import { useField, useForm } from "vee-validate";

import axios from "axios";
import { iOptions } from "@/types/options";

export default defineComponent({
  name: "AppForm",
  setup() {
    const options = ref<iOptions>({
      dataLayer: "",
      primaryColor: "",
      borderRadius: 0,
      dismissable: false,
      dismissType: "text",
      expiration: 365,
      closeType: "cross",
    });
    const urlOppions =
      "https://fealisher.blob.core.windows.net/alisher/assets/options.json";
    const auth =
      " https://fealisher.blob.core.windows.net/alisher?sp=racwl&st=2022-04-14T16:18:12Z&se=2022-04-22T00:18:12Z&spr=https&sv=2020-08-04&sr=c&sig=uF2wmDinnoK4NTfr1p%2B3qbMzS7YHo54fNuFbhlP8gf4%3D";

    const { handleSubmit } = useForm();
    const {
      value: dataLayer,
      errorMessage: dlError,
      handleBlur: dlBlur,
    } = useField("dataLayer", yup.string().trim().required());
    const {
      value: borderRadius,
      errorMessage: bError,
      handleBlur: bBlur,
    } = useField("borderRadius", yup.number().required());
    const {
      value: dismissable,
      errorMessage: dError,
      handleBlur: dBlur,
    } = useField("dismissable", yup.boolean().required());
    const {
      value: primaryColor,
      errorMessage: pError,
      handleBlur: pBlur,
    } = useField("primaryColor", yup.string().trim().required());
    const {
      value: expiration,
      errorMessage: eError,
      handleBlur: eBlur,
    } = useField("expiration", yup.number().required());
    const {
      value: closeType,
      errorMessage: cError,
      handleBlur: cBlur,
    } = useField("closeType", yup.string().trim().required());
    const {
      value: dismissType,
      errorMessage: dtError,
      handleBlur: dtBlur,
    } = useField("dismissType", yup.string().trim().required());

    const onSubmit = handleSubmit(async (values) => {
      try {
        // const data = options.value;
        console.log(values);
        // options.value = values;

        const config = {
          headers: {
            "x-ms-blob-type": "BlockBlob",
          },
        };

        await axios.put(urlOppions, values, config);
      } catch (error) {
        console.log(error);
      }
    });

    const fetchOptions = async () => {
      const { data } = await axios.get(urlOppions);
      options.value = data;
      console.log(data);
    };
    fetchOptions();

    const updateForm = async () => {
      const data = options.value;
      console.log(data);

      const config = {
        headers: {
          "x-ms-blob-type": "BlockBlob",
        },
      };

      await axios.put(urlOppions, data, config);
    };

    return {
      dataLayer,
      primaryColor,
      borderRadius,
      dismissable,
      dismissType,
      expiration,
      closeType,
      onSubmit,
      dlError,
      dlBlur,
      dError,
      dBlur,
      pError,
      pBlur,
      bError,
      bBlur,
      eError,
      eBlur,
      cError,
      cBlur,
      dtError,
      dtBlur,
      options,
    };
  },
});
</script>
