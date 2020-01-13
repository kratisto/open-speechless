<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      Open Speechless
    </v-app-bar>

    <v-content>
      <SpeechlessPage/>
    </v-content>
  </v-app>
</template>

<script>

import SpeechlessPage from "./components/speechless_page/SpeechLessPage";
export default {
  name: 'App',
  props: {
    source: String,
  },
  data: () => ({
    drawer: null,
  }),
  components: {
    SpeechlessPage
  },
};
</script>
