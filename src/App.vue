<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      Open Speechless
    </v-app-bar>

    <v-content>
      <SpeechlessPage/>
      <SpinningWheel/>
    </v-content>
  </v-app>
</template>

<script>

import SpeechlessPage from "./components/speechless_page/SpeechLessPage";
import SpinningWheel from "./components/spinning_wheel/SpinningWheel";
export default {
  name: 'App',
  props: {
    source: String,
  },
  data: () => ({
    drawer: null,
  }),
  components: {
    SpinningWheel,
    SpeechlessPage
  },
};
</script>
