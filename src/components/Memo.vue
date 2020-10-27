<template>
  <v-container fluid>
    <div class="buttonArea">
        <v-btn
          color="amber darken-2"
          dark
          @click="showForm = !showForm"
        >新しいメモを追加する</v-btn>
    </div>
    <v-expand-transition>
      <form @submit.prevent="submit" v-show="showForm">
        <v-row align="center" justify="center">
            <v-col cols="3">
              <v-combobox
                v-model="tagValues"
                :items="tagList"
                multiple
                chips
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
            <v-col cols="8" id="content-input">
              <v-text-field
                v-model="memoTitleValue"
                label="Memo Title"
                :error-messages="error"
                :append-outer-icon="'mdi-plus-circle'"
                @click:append-outer="addMemo"
              ></v-text-field>
            </v-col>
        </v-row>
      </form>
    </v-expand-transition>
    <v-row
      v-for="item in memos"
      :key="item.title"
      justify="center"
    >
      <v-col class="col-md-11 col-xl-10">
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
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Memo',
  props: ["db","tagList"],
  data () {
    return {
      tagValues: '',
      memoTitleValue: '',
      memos: [],
      error: '',
      showForm: false
    }
  },
  methods: {
    addMemo: async function() {
      if (!this.memoTitleValue) {
        this.error = 'メモ名は必須です';
        return;
      }

      let memoObj =  {
        title: this.memoTitleValue,
        tags: this.tagValues,
        text: ''
      };

      this.error = "";
      this.db.memos.add(memoObj);
      let allMemos = await this.db.memos.toArray();
      this.memos = allMemos;

      this.memoTitleValue = "",
      this.tagValues = ""
    },
    deleteMemo: async function(item) {
      this.db.memos.delete(item.id);
      let allMemos = await this.db.memos.toArray();
      this.memos = allMemos;
    },
    onChange: function(item) {
      let memoValue = event.target.value;
      this.db.memos.put({
        id: item.id,
        title: item.title,
        tags: item.tags,
        text: memoValue
      })
    }
  },
  async created () {
    let allMemos = await this.db.memos.toArray();
    this.memos = allMemos;
  }
}
</script>

<style>

</style>