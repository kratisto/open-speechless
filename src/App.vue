<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      Open Speechless
    </v-app-bar>

    <v-content>
      <SpeechlessSubject/>
      <SpinningWheel/>
    </v-content>
  </v-app>
</template>

<script>

import SpinningWheel from "./components/spinning_wheel/SpinningWheel";
import SpeechlessSubject from "./components/speechless_subject/SpeechLessSubject";
export default {
  name: 'App',
  props: {
    source: String,
  },
  data: () => ({
    drawer: null,
  }),
  components: {
    SpeechlessSubject,
    SpinningWheel
  },
};
</script>
