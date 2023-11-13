<script setup>
import VolunteerStatus from "./components/VolunteerStatus.vue"

</script>

<template>
  <VApp>
    <v-app-bar
      color="primary"
    >
      <v-app-bar-title>BBBS Volunteer Status Check</v-app-bar-title>

    </v-app-bar>
    <!-- SIDEBAR -->
    <VMain>
      <volunteer-status />
    </VMain>
  </VApp>
</template>

