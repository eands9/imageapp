<script setup>
import { reactive } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { email, required } from "@vuelidate/validators";

const initialState = {
  gender: "",
  email: "",
  id: "",
  showProgress: false,
};

const state = reactive({
  ...initialState,
});

const rules = {
  gender: { required },
  email: { required, email },
};

// const showProgress = ref(true)

const v$ = useVuelidate(rules, state);

function clear() {
  v$.value.$reset();

  for (const [key, value] of Object.entries(initialState)) {
    state[key] = value;
  }
}
function submitForm() {
    state.showProgress=true
  v$.value.$validate();
  if (!v$.value.$error) {
    console.log("There's data...");
    getData()
  } else {
    console.log("There's no data...");
  }


}
async function getData(){
    // const id = state.gender;

    // const gql = `
    // query getById($id: ID!) {
    //   imageapp_by_pk(id: $id) {
    //     id,
    //     email,
    //     gender
    //   }
    // }`;

    // const query = {
    //   query: gql,
    //   variables: {
    //     id: id,
    //   },
    // };

    const email = state.email;

    const gql = `
    query getById($id: ID!) {
      imageapp_by_pk(id: $id) {
        id,
        email,
        gender
      }
    }`;
    const query = {
      query: gql,
      variables: {
        email: email,
      },
    };

    const endpoint = "/data-api/graphql";
    const response = await fetch(endpoint, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(query),
    });
    const result = await response.json();
    console.table(result.data.imageapp_by_pk);
    // console.log(result.data.imageapp_by_pk.gender);
    state.showProgress = false;

}
</script>
<template>
  <v-overlay v-model="state.showProgress" class="justify-center align-center">
    <v-progress-circular
      indeterminate
      size="60"
      width="5"
      color="primary"
    ></v-progress-circular>
  </v-overlay>

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

      <v-textarea class="mt-15" v-show="false" variant="outlined"></v-textarea>
    </form>
  </v-sheet>
</template>
