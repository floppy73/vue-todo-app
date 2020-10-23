<template>
  <v-card flat>
    <v-card-text>
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
                elevation="0"
                color="grey darken-1"
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
      tasks: [
        {content: '数学の授業を一コマ見る', tags: ['Study', 'Math'], isChecked: false},
        {content: '書類を本校に送る', tags: ['Work'], isChecked: false}
      ]
    }
  },
  methods: {

  }
}
</script>

<style lang="scss">
tr.done {
  background-color: #e7e7e7;

  td:nth-child(2){text-decoration: line-through;}
}
</style>
