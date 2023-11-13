<script setup>
import { reactive } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { email, required } from "@vuelidate/validators";

const initialState = {
  gender: "",
  email: "",
};

const state = reactive({
  ...initialState,
});

const rules = {
  gender: { required },
  email: { required, email },
};

const v$ = useVuelidate(rules, state);

function clear() {
  v$.value.$reset();

  for (const [key, value] of Object.entries(initialState)) {
    state[key] = value;
  }
}
async function submitForm() {
  v$.value.$validate();
//   if(!v$.value.$error){
//     console.log("There's data...")
//   } else {
//     console.log("There's no data...")
//     state.gender = 'm'
//   }

  const query = `
      {
        imageapp_by_pk(id: "1001") {
            id,
            email,
            gender
        }
      }`;

      const endpoint = "/data-api/graphql";
      const response = await fetch(endpoint, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query: query }),
      });
      const result = await response.json();
      console.table(result.data.imageapps.items);
  
}
</script>

<template>
  <v-sheet width="300" class="mx-auto mt-15">
    <form>
      <v-text-field
        v-model="state.gender"
        :error-messages="v$.gender.$errors.map((e) => e.$message)"
        :counter="10"
        label="Gender"
        variant="outlined"
        required
        @input="v$.gender.$touch"
        @blur="v$.gender.$touch"
      ></v-text-field>

      <v-text-field
        v-model="state.email"
        :error-messages="v$.email.$errors.map((e) => e.$message)"
        label="E-mail"
        variant="outlined"
        required
        @input="v$.email.$touch"
        @blur="v$.email.$touch"
      ></v-text-field>

      <v-btn class="me-4 mt-5" @click="submitForm"> submit </v-btn>
      <v-btn class="mt-5" @click="clear"> clear </v-btn>

      <v-textarea class="mt-15" label="Label" variant="outlined"></v-textarea>
    </form>
  </v-sheet>
</template>
