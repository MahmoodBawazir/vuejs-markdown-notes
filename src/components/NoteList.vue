<template>
  <div class="notes">
    <Note
      v-for="(note, index) in notes"
      :key="index"
      :note="note"
      :index="index"
      :activeEdit="activeEdit"
      @editNote="editNote"
      @deleteNote="deleteNote"
      @noteEdited="noteEdited"
      @textareaAdjust="textareaAdjust"
    />
  </div>
</template>

<script>
import Note from './Note'

export default {
  props: ['notes', 'activeEdit'],
  components: {
    Note
  },
  methods: {
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
  }
}
</script>
