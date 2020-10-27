<template>
  <v-card flat>
    <v-card-text>
      <div class="buttonArea">
        <v-btn
          color="amber darken-2"
          dark
          @click="showForm = !showForm"
        >新しい締切を追加する</v-btn>
      </div>
      <v-expand-transition>
        <form @submit.prevent="submit" v-show="showForm">
          <v-container fluid>
            <v-row align="center" justify="center">
              <v-col cols="2">
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
              <v-col cols="7">
                <v-text-field
                  :error-messages="contentError"
                  v-model="deadlineValue"
                  label="Content"
                ></v-text-field>
              </v-col>
              <v-col id="content-input">
                <v-menu
                  v-model="dateValue"
                  :close-on-content-click="false"
                  transition="fade-transition"
                  offset-y
                  min-width="290px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="datePick"
                      label="Deadline"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                      :error-messages="deadlineError"
                      prepend-inner-icon="mdi-calendar"
                      :append-outer-icon="'mdi-plus-circle'"
                      @click:append-outer="addDeadline"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    color="teal"
                    v-model="datePick"
                    @input="dateValue = false"
                  ></v-date-picker>
                </v-menu>
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
                  <th class="text-left">
                    内容
                  </th>
                  <th class="text-left">
                    期限
                  </th>
                  <th class="text-left">
                    タグ
                  </th>
                  <th></th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="item in deadlines"
                  :key="item.content"
                  class="body-1"
                >
                  <td class="body-1">{{ item.content }}</td>
                  <td class="body-1">{{ item.date }}</td>
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
                      @click="deleteDeadline(item)"
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
  name: 'Deadlines',
  props: ["db","tagList"],
  data () {
    return {
      done: '',
      tagValues: [],
      deadlineValue: '',
      dateValue: '',
      datePick: '',
      deadlines: [],
      contentError: '',
      deadlineError: '',
      showForm: false
    }
  },
  methods: {
    addDeadline: async function() {
      if (!this.deadlineValue) {
        this.contentError = 'メモ名は必須です';
        return;
      } else if (!this.datePick) {
        this.deadlineError = '締め切りは必須です';
        return;
      }

      let deadlineObj =  {
        content: this.deadlineValue,
        tags: this.tagValues,
        date: this.datePick
      };

      this.contentError = '';
      this.deadlineError = '';

      this.db.deadlines.add(deadlineObj);
      let allDeadlines = await this.db.deadlines.toArray();
      this.deadlines = allDeadlines;

      this.tagValues = "",
      this.deadlineValue = "";
      this.datePick = ""
    },
    deleteDeadline: async function(item) {
      this.db.deadlines.delete(item.id);
      let allDeadlines = await this.db.deadlines.toArray();
      this.deadlines = allDeadlines;
    }
  },

  async created () {
    let allDeadlines = await this.db.deadlines.toArray();
    this.deadlines = allDeadlines;
  }
}
</script>

<style>

</style>