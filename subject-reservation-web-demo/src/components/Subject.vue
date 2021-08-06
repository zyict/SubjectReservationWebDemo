<template>
  <div>
    <v-card class="pa-md-0 mx-auto" max-width="344">
      <v-container fluid>
        <v-layout column>
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
            height="200px"
          ></v-img>

          <v-card-title>
            {{ subject.subjectId }} <br />
            {{ subject.subjectName }}
          </v-card-title>
          <v-card-subtitle>
            <p>
              Maximum: {{ subject.quota }} | Current:
              {{ subject.currentStudentNumber }}
            </p>
          </v-card-subtitle>
          <v-card-actions>
            <v-btn color="orange lighten-2" text @click="reserveSubject()">
              Reserve
            </v-btn>
          </v-card-actions>
        </v-layout>
      </v-container>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
//import { fetchSubjectList } from "../service/subject";
export default {
  data: () => ({
    show: false,
  }),
  props: {
    subject: {
      type: Object,
      default: () => ({
        subjectId: "int101",
        quota: 30,
        currentStudentNumber: 0,
        subjectName: "Introduction to Java 1",
      }),
    },
  },
  methods: {
    reserveSubject: async function () {
      const loader = this.$loading.show();
      try {
        await axios.post(`subject/${this.subject.subjectId}/reserve`);
      } catch (err) {
        console.error(err.message);
        this.$emit("subjectIsMax");
      }
      this.$emit("fetchSubjects");
      loader.hide();
    },
  },
};
</script>