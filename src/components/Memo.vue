<template>
  <v-container fluid>
    <form @submit.prevent="submit">
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
              v-model="memoTitleValue"
              label="Memo Title"
              :append-outer-icon="'mdi-plus-circle'"
              @click:append-outer="addMemo"
            ></v-text-field>
          </v-col>
      </v-row>
    </form>
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
          <v-chip
            v-for="tag in item.tags"
            :key="tag"
            class="ma-1"
            color="teal lighten-5"
          >
            {{ tag }}
          </v-chip>
          <v-spacer></v-spacer>
          <v-btn
            fab
            dark
            small
            elevation="1"
            color="grey darken-1"
            @click="deleteMemo(item)"
          >
            <v-icon fab dark>mdi-delete</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-textarea
            filled
            :value="item.text"
            @change="onChange(item)"
            background-color="grey lighten-3"
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
      tagList: ['Work', 'Study', 'Math'],
      tagValues: '',
      memoTitleValue: '',
      memos: [
        {title: 'Math', tags: ['Study'], text: 'ベクトル復習する'},
        {title: 'その他', tags: ['Hobby'], text: 'アイマスを見る'}
      ]
    }
  },
  methods: {
    addMemo: function() {
      let memoObj =  {
        title: this.memoTitleValue,
        tags: this.tagValues,
        text: ''
      };
      this.memos.push(memoObj);
      this.memoTitleValue = "",
      this.tagValues = ""
    },
    deleteMemo: function(item) {
      let index = this.memos.indexOf(item);
      this.memos.splice(index, 1);
    },
    onChange: function(item) {
      item.text = event.target.value;
    }
  }
}
</script>

<style>

</style>