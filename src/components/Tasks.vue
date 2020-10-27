<template>
  <v-card flat>
    <v-card-text>
      <div class="buttonArea">
        <v-btn
          color="amber darken-2"
          dark
          @click="showForm = !showForm"
        >新しいタスクを追加する</v-btn>
      </div>
      <v-expand-transition>
        <form @submit.prevent="submit" v-show="showForm">
          <v-container>
            <v-row align="center" justify="center">
              <v-col cols="3" class="col-lg-2">
                <v-combobox
                  v-model="tagValues"
                  :items="tagList"
                  multiple
                  chips
                  deletable-chips
                  label="Tags"
                  prepend-inner-icon="mdi-tag"
                >
                  <template v-slot:selection="data">
                    <v-chip
                      color="teal lighten-5"
                      :key="JSON.stringify(data.item)"
                      v-bind="data.attrs"
                      :input-value="data.selected"
                      :disabled="data.disabled"
                      @click:close="data.parent.selectItem(data.item)"
                    >
                      {{ data.item }}
                    </v-chip>
                  </template>
                </v-combobox>
              </v-col>
              <v-col cols="8" class="col-md-6 col-xl-5" id="content-input">
                <v-text-field
                  v-model="taskValue"
                  label="Task"
                  :error-messages="error"
                  :append-outer-icon="'mdi-plus-circle'"
                  @click:append-outer="addTask"
                  hint="右の+を押して追加"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </form>
      </v-expand-transition>
      <v-divider></v-divider>
      <v-container>
        <v-row justify="center">
          <v-col class="
            col-md-11
            col-xl-10
          ">
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
                  <td>
                    <v-checkbox
                      v-model="item.isChecked"
                      color="teal"
                    ></v-checkbox></td>
                  <td class="body-1">{{ item.content }}</td>
                  <td class="body-1">
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
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: 'Tasks',
  props: ["db","tagList"],
  data () {
    return {
      done: '',
      tagValues: [],
      taskValue: '',
      tasks: [],
      error: '',
      showForm: false
    }
  },
  methods: {
    addTask: async function () {
      if (!this.taskValue) {
        this.error = 'タスク名は必須です';
        return;
      }
      let taskObj =  {
        content: this.taskValue,
        tags: this.tagValues,
        isChecked: false
      };
      this.error = "";
      this.db.tasks.add(taskObj);
      let allTasks = await this.db.tasks.toArray();
      this.tasks = allTasks;
      this.tagValues = "";
      this.taskValue = ""
    },
    deleteTask: async function(item) {
      this.db.tasks.delete(item.id);
      let allTasks = await this.db.tasks.toArray();
      this.tasks = allTasks;
    }
  },

  async created () {
    let allTasks = await this.db.tasks.toArray();
    this.tasks = allTasks;
  }
}
</script>

<style lang="scss">

</style>
