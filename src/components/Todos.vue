<template>
  <v-card flat>
    <v-card-text>
      <v-simple-table>
        <thead>
          <tr>
            <th class="text-left">
            </th>
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
            v-for="item in todos"
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
  name: 'Todos',
  data () {
    return {
      done: '',
      todos: [
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

</style>
