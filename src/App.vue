<template>
  <v-app>
    <v-card
      tile>
      <v-toolbar
        color="teal"
        dark
        flat
      >
        <v-toolbar-title>TODO APP</v-toolbar-title>
        <template v-slot:extension>
          <v-tabs 
            v-model="tab"
            fixed-tabs
            centered
          >
            <v-tabs-slider color="teal lighten-4"></v-tabs-slider>
            <v-tab
              v-for="item in items"
              :key="item.tab">
              {{ item.tab }}
            </v-tab>
          </v-tabs>
        </template>
      </v-toolbar>
    </v-card>
    <v-main>
    <v-tabs-items v-model="tab">
        <v-tab-item
          v-for="item in items"
          :key="item.tab">
              <component v-bind:is="item.content" :db="db" :tagList="tagList"></component>
        </v-tab-item>
      </v-tabs-items>
    </v-main>
  </v-app>
</template>

<script>
import Dexie from 'dexie';
import Tasks from './components/Tasks';
import Deadlines from './components/Deadlines';
import Memo from './components/Memo';

export default {
  name: 'App',

  components: {
    Tasks,
    Deadlines,
    Memo
  },

  data () {
    return {
      db: new Dexie('todo_app_db'),
      tab: null,
      items: [
        { tab: 'Tasks', content: 'Tasks'},
        { tab: 'Deadlines', content: 'Deadlines'},
        { tab: 'Memo', content: 'Memo'},
      ],
      tagList: ['Work', 'Study', 'Hobby', 'Others'],
    }
  },

  created () {
    console.log('created');
    this.db.version(1).stores({
      tasks: '++id',
      deadlines: '++id',
      memos: '++id'
    });
  }
};
</script>

<style lang="scss">
div.v-tabs-slider-wrapper {
  height: 4px !important;
}
tr.done {
  background-color: #e7e7e7;

  td:nth-child(2){text-decoration: line-through;}
}

.mdi-plus-circle:before {
  font-size: 40px;
  color: #FFA726;
}
div.v-input__slot > div {
  margin: 0 auto !important;
}
</style>
