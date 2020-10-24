<template>
  <v-container fluid>
    <v-row
      v-for="item in memos"
      :key="item.title"
    >
      <v-card
        flat
        width="90%"
        class="mx-auto"
      >
        <v-card-title
          class="font-weight-regular"
        >
          {{ item.title }}
          <v-spacer></v-spacer>
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
        </v-card-title>
        <v-card-text>
          <v-textarea
            filled
            :value="item.text"
          ></v-textarea>
        </v-card-text>
        <v-divider></v-divider>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Memo',
  data () {
    return {
      memos: [
        {title: 'Math', text: 'ベクトル復習する'},
        {title: 'その他', text: 'アイマスを見る'}
      ]
    }
  }
}
</script>

<style>

</style>