<template>
  <v-app>
    <v-main>
      <ViewWrap />
    </v-main>
  </v-app>
</template>

<script>
import ViewWrap from "./components/ViewWrap";

export default {
  name: "App",

  components: {
    ViewWrap,
  },
};
</script>
