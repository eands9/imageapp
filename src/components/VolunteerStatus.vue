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
function submitForm() {
  v$.value.$validate();
  if(!v$.value.$error){
    console.log("There's data...")
  } else {
    console.log("There's no data...")
  }
  
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
        required
        @input="v$.gender.$touch"
        @blur="v$.gender.$touch"
      ></v-text-field>

      <v-text-field
        v-model="state.email"
        :error-messages="v$.email.$errors.map((e) => e.$message)"
        label="E-mail"
        required
        @input="v$.email.$touch"
        @blur="v$.email.$touch"
      ></v-text-field>

      <v-btn class="me-4" @click="submitForm"> submit </v-btn>
      <v-btn @click="clear"> clear </v-btn>
    </form>
  </v-sheet>
</template>
