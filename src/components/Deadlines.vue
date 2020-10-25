<template>
  <v-card flat>
    <v-card-text>
      <form @submit.prevent="submit">
        <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="2">
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
            <v-col cols="7">
              <v-text-field
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
      <v-divider></v-divider>
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
          >
            <!--<td class="text-right">
              <v-checkbox
                v-model="item.isChecked"
                color="teal"
              ></v-checkbox></td>-->
            <td>{{ item.content }}</td>
            <td class="body-1">{{ item.date }}</td>
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
                @click="deleteDeadline(item)"
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
  name: 'Deadlines',
  props: ["db","tagList"],
  data () {
    return {
      done: '',
      tagValues: [],
      deadlineValue: '',
      dateValue: '',
      datePick: '',
      deadlines: []
    }
  },
  methods: {
    addDeadline: async function() {
      let deadlineObj =  {
        content: this.deadlineValue,
        tags: this.tagValues,
        date: this.datePick
      };
      this.db.deadlines.add(deadlineObj);
      let allDeadlines = await this.db.deadlines.toArray();
      this.deadlines = allDeadlines;
      this.tagValues = "",
      this.deadlineValue = "";
      this.datePick = ""
    },
    deleteDeadline: async function(item) {
      let index = this.deadlines.indexOf(item);
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