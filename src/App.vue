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
        <v-tabs v-model="tab">
          <v-tab
            v-for="item in items"
            :key="item.tab">
            {{ item.tab }}
          </v-tab>
        </v-tabs>
      </v-toolbar>
    </v-card>
    <v-main>
    <v-tabs-items v-model="tab">
        <v-tab-item
          v-for="item in items"
          :key="item.tab">
              <component v-bind:is="item.content"></component>
        </v-tab-item>
      </v-tabs-items>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Todos from './components/Todos';
import Deadlines from './components/Deadlines';
import Memo from './components/Memo';

export default {
  name: 'App',

  components: {
    HelloWorld,
    Todos,
    Deadlines,
    Memo
  },

  data: () => ({
    tab: null,
    items: [
      { tab: 'Todos', content: 'Todos'},
      { tab: 'Deadlines', content: 'Deadlines'},
      { tab: 'Memo', content: 'Memo'},
    ]
  }),
};
</script>
