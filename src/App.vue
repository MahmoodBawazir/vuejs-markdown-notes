<template>
  <div id="app">
    <header class="header">
      <h1>VueJS Markdown Notes</h1>

      <span class="icons">
        <i class="fa fa-plus-circle fa-2x" @click="addNote"></i>
      </span>
    </header>

    <NoteList
      :notes="notes"
      :activeEdit="activeEdit"
      @editNote="editNote"
      @deleteNote="deleteNote"
      @noteEdited="noteEdited"
      @textareaAdjust="textareaAdjust"
    />

    <p class="small">
      <em>
        VueJS version of
        <a
          href="https://codepen.io/nickmoreton/full/gbyygq/"
          target="_blank"
        >AngularJS Markdown Notes App by Nick Moreton</a>
      </em>
    </p>
  </div>
</template>

<script>
import NoteList from './components/NoteList'

const STORAGE_KEY = 'notes-vuejs-2.6.10'
const defaultNote = [
  {
    id: 1,
    text:
      '# Hello, World!\n\nThis is your first VueJS Markdown note. You can:\n\n* Double Click to edit\n\n* Click off/Blur to save your changes(uses localStorage)\n\n* Add a new note  by clicking the plus sign above.\n\n* Delete this note\n\nMarkdown compiled using the fantastic [marked](https://marked.js.org/#/README.md#README.md) compiler.',
    createdOn: 1557729867936
  }
]

const noteStorage = {
  fetch() {
    var notes = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || JSON.stringify(defaultNote)
    )
    notes.forEach(function(note, index) {
      note.id = index
    })
    noteStorage.uid = notes.length
    return notes
  },
  save(notes) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(notes))
  }
}

export default {
  data() {
    return {
      notes: noteStorage.fetch(),
      activeEdit: ''
    }
  },
  components: {
    NoteList
  },
  watch: {
    notes: {
      handler: function(notes) {
        noteStorage.save(notes)
      },
      deep: true
    }
  },
  methods: {
    addNote() {
      this.notes.push({
        id: noteStorage.uid++,
        text: '',
        createdOn: Date.now()
      })
    },
    editNote(note) {
      this.activeEdit = note
    },
    cancelEdit(note) {
      this.activeEdit = null
    },
    noteEdited(note) {
      if (!this.activeEdit) return

      this.activeEdit = null
      note.text = note.text.trim()

      if (!note.text) {
        this.deleteNote(note)
      }
    },
    deleteNote(note) {
      this.notes.splice(this.notes.indexOf(note), 1)
    },
    textareaAdjust(note, index) {
      let textareaEl = document
        .querySelectorAll('.note')
        [index].querySelector('textarea')

      textareaEl.style.height = '1px'
      textareaEl.style.height = 25 + textareaEl.scrollHeight + 'px'
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro:400,700');
@import url('https://fonts.googleapis.com/css?family=Source+Code+Pro');
@import url('https://s3-us-west-2.amazonaws.com/s.cdpn.io/110131/githubmd.css');
@import url('https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css');

* {
  box-sizing: border-box;
}

body {
  max-width: 580px;
  margin: auto;
  padding: 10px;
  position: relative;
  background: #fdfdfd;
}

html,
body,
.editor {
  margin: 0;
  height: 100%;
}

html,
body,
.compiledMarkdown {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  color: #333;
}

img {
  max-width: 100%;
  height: auto;
}

textarea {
  font-size: 1.2em;
  font-family: 'Source Sans Pro', sans-serif;
  border: none;
  box-shadow: none;
  margin: 0;
  padding: 10px 20px 10px;
  background: #f9f9f9;

  width: 100%;
  height: 100%;
  min-height: 200px;

  outline: none;
  resize: none;

  &:focus {
    font-family: 'Source Code Pro', courier, monospace;
    font-size: 1em;
  }
}

.editor {
  position: relative;
  background: #f9f9f9;
  min-height: 200px;

  display: flex;
  flex-wrap: wrap;

  div {
    width: 100%;
  }

  .compiledMarkdown {
    cursor: text;
    padding-left: 20px;
  }
}

.header {
  margin: 5px 10px 0;
  color: #d46e58;
  display: inline-block;

  h1 {
    margin: 0;
  }
}

.note {
  background: #eee;
  box-shadow: #999 1px 1px 3px;
  margin: 30px 0;
  padding-top: 40px;
  min-height: 200px;
  width: 100%;
  display: block;
  position: relative;
  overflow: hidden;
}

.date {
  position: absolute;
  top: 0;
  left: 0;
  padding: 15px;
  font-size: 0.7em;
  font-style: italic;
  color: #71CBD0;
}

.icons {
  position: absolute;
  right: 0;
  top: 0;
  padding: 10px;

  .fa:not(:first-child) {
    margin-left: 8px;
  }
}

.fa-plus-circle,
.fa-trash-o {
  cursor: pointer;
}

.fa-check {
  color: #92D788;
}

.fa-trash-o {
  color: #C2474B;

  &:hover {
    color: rgba(194, 71, 75, 0.5);
  }
}

.fa-pencil {
  color: #DBC394;
}

.fa-plus-circle {
  color: #71CBD0;

  &:hover {
    color: rgba(113, 203, 208, 0.5);
  }
}

.small {
  font-size: 12px;
}
</style>
