<template>
  <div class="note">
    <span class="date">{{ note.createdOn | formatDate }}</span>
    
    <span class="icons">
      <i class="fa fa-pencil fa-lg" v-if="note == activeEdit"></i>
      <i class="fa fa-check fa-lg" v-else></i>
      
      <i class="fa fa-trash-o fa-lg" @click="deleteNote(note)"></i>
    </span>

    <div class="editor" @dblclick="editNote(note)" title="double click to edit">
      <textarea
        type="text"
        v-model="note.text"
        v-focus="note == activeEdit"
        v-if="note == activeEdit"
        @blur="noteEdited(note)"
        @update="update"
        @focus="textareaAdjust(note, index)"
        @keyup="textareaAdjust(note, index)"
      ></textarea>
      <div v-html="compiledMarkdown" v-else class="compiledMarkdown"></div>
    </div>
  </div>
</template>

<script>
import marked from 'marked'
import _ from 'lodash'
import moment from 'moment'

export default {
  props: ['note', 'index', 'activeEdit'],
  methods: {
    update: _.debounce(function(e) {
      this.note.text = e.target.value
    }, 300),
    editNote(note) {
      this.$emit('editNote', note)
    },
    deleteNote(note) {
      this.$emit('deleteNote', note)
    },
    noteEdited(note) {
      this.$emit('noteEdited', note)
    },
    textareaAdjust(note, index) {
      this.$emit('textareaAdjust', note, index)
    }
  },
  computed: {
    compiledMarkdown() {
      return marked(this.note.text, { sanitize: true })
    }
  },
  filters: {
    formatDate(value) {
      return moment(value).format('LLLL')
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.select()
      }
    }
  }
}
</script>
