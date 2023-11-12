<template>
  <VApp>
    <v-app-bar
      color="primary"
      density="compact"
    >

    </v-app-bar>
    <!-- SIDEBAR -->
    <VMain>
      <!-- CARDS -->
    </VMain>
  </VApp>
</template>

<script>
export default {
  data() {
    return {
      students: [],
    };
  },
  mounted() {
    this.getUpdates();
  },
  methods: {
    async getUpdates() {
      // const endpoint='/data-api/graphql/Update'
      // const response = await fetch(endpoint)
      // const data = await response.json()
      // console.log(data)

      const query = `
      {
        imageapps {
          items{
            email,
            gender
          },
        endCursor,
        hasNextPage
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
    },
  },
};
</script>
