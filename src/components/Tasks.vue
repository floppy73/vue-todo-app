<template>
  <v-card flat>
    <v-card-text>
      <form @submit.prevent="submit">
        <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="3">
              <v-autocomplete
                v-model="tagValues"
                :items="tagList"
                multiple
                chips
                deletable-chips
                label="Tags"
                prepend-inner-icon="mdi-tag"
              ></v-autocomplete>
            </v-col>
            <v-col cols="8" id="content-input">
              <v-text-field
                v-model="taskValue"
                label="Task"
                :append-outer-icon="'mdi-plus-circle'"
                @click:append-outer="addTask"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </form>
      <v-divider></v-divider>
      <v-simple-table>
        <thead>
          <tr>
            <th></th>
            <th class="text-left">
              内容
            </th>
            <th class="text-left">
              タグ
            </th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in tasks"
            :key="item.content"
            v-bind:class="{done: item.isChecked}"
          >
            <td class="text-right">
              <v-checkbox
                v-model="item.isChecked"
                color="teal"
              ></v-checkbox></td>
            <td>{{ item.content }}</td>
            <td>
              <v-chip
                v-for="tag in item.tags"
                :key="tag"
                class="ma-1"
                color="teal lighten-5"
              >
                {{ tag }}
              </v-chip>
            </td>
            <td>
              <v-btn
                fab
                dark
                small
                elevation="1"
                color="grey darken-1"
                @click="deleteTask(item)"
              >
                <v-icon fab dark>mdi-delete</v-icon>
              </v-btn>
            </td>
          </tr>
        </tbody>
      </v-simple-table>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: 'Tasks',
  data () {
    return {
      done: '',
      tagList: ['Work', 'Study', 'Math'],
      tagValues: [],
      taskValue: '',
      tasks: [
        {content: '数学の授業を一コマ見る', tags: ['Study', 'Math'], isChecked: false},
        {content: '書類を本校に送る', tags: ['Work'], isChecked: false}
      ]
    }
  },
  methods: {
    addTask: function() {
      let taskObj =  {
        content: this.taskValue,
        tags: this.tagValues,
        isChecked: false
      };
      this.tasks.push(taskObj);
      this.tagValues = "",
      this.taskValue = ""
    },
    deleteTask: function(item) {
      let index = this.tasks.indexOf(item);
      this.tasks.splice(index, 1);
    }
  }
}
</script>

<style lang="scss">

</style>
