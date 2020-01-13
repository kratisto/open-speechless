<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      Open Speechless
    </v-app-bar>

    <v-content>
      <SpeechlessSubject @inputData="updateSubjects"/>
      <SpinningWheel :subjects="subjects"/>
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
    subjects: [],
  }),
  methods: {
    updateSubjects(subjects) {
      this.subjects=subjects;
    }
  },
  components: {
    SpeechlessSubject,
    SpinningWheel
  },
};
</script>
