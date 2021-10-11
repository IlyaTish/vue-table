<template>
  <div id="app">
    <v-app id="inspire">
      <v-main>
        <v-container fluid>
          <table-data
            v-if="tableData.length > 0"
            :data="tableData"
          />
        </v-container>
      </v-main>
    </v-app>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios';
import 'vuetify/dist/vuetify.min.css';
import TableData from '@/components/TableData/TableData.vue';

@Component({
  components: {
    TableData,
  },
})

export default class App extends Vue {
  tableData: Array<string> = [];

  async mounted(): Promise<void> {
    await axios
      .get('https://jsonplaceholder.typicode.com/posts?_limit=50')
      .then((response) => {
        this.tableData = response.data;
      })
      .catch((e) => {
        console.log(e);
      });
  }
}
</script>

<style lang="scss">
.v-application {
  &--wrap {
    background-color: #efefef;
  }
}
</style>
