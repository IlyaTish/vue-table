<template>
  <v-sheet
    color="white"
    elevation="1"
  >
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th
              v-for="(th, index) in thead"
              :class="['th', sortBy === th ? sortDirection : '']"
              :key='`th-${index}`'
              @click="sort(th)"
            >
              <div class="th__wrapper">
                <span>{{ th }}</span>
                <!-- eslint-disable max-len -->
                <img class="arrow-icon" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAzMzAgMzMwIiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCAzMzAgMzMwIiB4bWw6c3BhY2U9InByZXNlcnZlIj48cGF0aCBkPSJtMzI1LjYwNiAyMjkuMzkzLTE1MC4wMDQtMTUwYTE0Ljk5NyAxNC45OTcgMCAwIDAtMjEuMjEzLjAwMWwtMTQ5Ljk5NiAxNTBjLTUuODU4IDUuODU4LTUuODU4IDE1LjM1NSAwIDIxLjIxMyA1Ljg1NyA1Ljg1NyAxNS4zNTUgNS44NTggMjEuMjEzIDBsMTM5LjM5LTEzOS4zOTMgMTM5LjM5NyAxMzkuMzkzQTE0Ljk1MyAxNC45NTMgMCAwIDAgMzE1IDI1NWExNC45NSAxNC45NSAwIDAgMCAxMC42MDctNC4zOTRjNS44NTctNS44NTggNS44NTctMTUuMzU1LS4wMDEtMjEuMjEzeiIvPjwvc3ZnPg==">
              </div>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="(tr, index) in trToDisplay"
            :key='`tr-${index}`'
          >
            <td
              v-for="(td, index) in tr"
              :key='`td-${index}`'
            >{{ td }}</td>
          </tr>
        </tbody>
      </template>

      <template v-slot:bottom>
        <div class="v-data-footer">
          <v-btn
            color="primary"
            elevation="2"
            @click="showMore"
          >Показать еще</v-btn>

          <div class="v-data-footer__select">
            <v-select
              v-model="currentEntrie"
              class="select"
              :items="allEntries"
              :menu-props="{ top: true }"
              :label="`${currentEntrie}`"
              hide-details
              single-line
              @change="paginate"
            />
          </div>
        </div>
      </template>
    </v-simple-table>
  </v-sheet>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class TableData extends Vue {
  @Prop({
    type: Array,
    required: true,
  }) data!: Array<string>;

  thead: Array<string> = [];

  totalData: number = this.data.length;

  currentEntrie = 10;

  allEntries: Array<number> = [10, 15, 20, 50];

  sortBy = 'id';

  sortDirection = 'asc';

  mounted(): void {
    this.thead = Object.keys(this.data[0]);
    // eslint-disable-next-line prefer-destructuring
    this.sortBy = this.thead[0];
  }

  get trToDisplay(): Array<unknown> {
    const items = this.data.slice(0, this.currentEntrie);

    // eslint-disable-next-line array-callback-return
    items.sort((a: string, b: string): number => {
      let modifier = 1;

      if (this.sortDirection === 'desc') modifier = -1;
      if (a[this.sortBy] < b[this.sortBy]) return -1 * modifier;
      if (a[this.sortBy] > b[this.sortBy]) return 1 * modifier;

      return 0;
    });
    return items;
  }

  paginate(curEntrie: number): void {
    this.currentEntrie = curEntrie;
  }

  showMore(): void {
    if (this.currentEntrie < this.totalData) {
      // eslint-disable-next-line no-unused-expressions
      this.currentEntrie += 10;
    }
  }

  sort(s: string): void {
    if (s === this.sortBy) {
      this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
    }
    this.sortBy = s;
  }
}
</script>

<style lang="scss">
.v-data-footer {
  &__select {
    .v-select.select {
      flex: 0 0 60px;
    }
  }
}

.th {
  &__wrapper {
    display: flex;
    align-items: center;
  }
}

.th.asc {
  .arrow-icon {
    opacity: 1;
  }
}

.th.desc {
  .arrow-icon {
    opacity: 1;
    transform: rotate(180deg);
  }
}

.arrow-icon {
  width: 10px;
  height: 10px;
  flex: 10px 0 0;
  margin: 0 0 0 6px;
  transition: .2s;
  opacity: 0;
}
</style>
