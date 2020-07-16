<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      Open Speechless
    </v-app-bar>
    <v-content>
      <SpeechlessSubject @subjects="updateSubjects" @replace-subject="replaceSubject" :subjects="subjects"/>
      <SpinningWheel ref="spinningWheel" :subjects="subjects"/>
    </v-content>
  </v-app>
</template>

<script>

  import SpinningWheel from "../components/spinning_wheel/SpinningWheel";
  import SpeechlessSubject from "../components/speechless_subject/SpeechLessSubject";

  export default {
    name: 'App',
    props: {
      source: String,
    },
    data: () => ({
      subjects: [],
    }),
    created: function () {
        var subjectsPath = this.$route.params.subjects
        if (subjectsPath !== undefined) {
          // eslint-disable-next-line no-console
          this.subjects = JSON.parse(atob(subjectsPath));
        } else {
          this.subjects = [
            {Nom:"Un film",  Lien:""},
            {Nom:"Une serie", Lien:""},
            {Nom:"Un logiciel", Lien:""}
          ]
        }
    },

    methods: {

      updateSubjects(subjects) {
        this.subjects=subjects;
      },
      replaceSubject(replace){
        Object.assign(this.subjects[replace.index], replace.subject)
        this.$refs.spinningWheel.replaceSubject(replace)
      }
    },
    components: {
      SpeechlessSubject,
      SpinningWheel
    },
  };
</script>
