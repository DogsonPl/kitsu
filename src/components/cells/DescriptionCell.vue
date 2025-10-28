<template>
  <td class="description-cell">
    <div
      v-if="!isEditing"
      class="description-full-text selectable"
      v-html="renderMarkdown(entry.description)"
      @click="onClickEdit"
    ></div>

    <textarea
      v-else
      class="description-editor"
      ref="text"
      :value="entry.description"
      @keyup.esc="onCancelEdit"
      @keyup.ctrl.enter="onSaveEdit"
      @blur="onSaveEdit"
    ></textarea>
  </td>
</template>

<script>
import { renderMarkdown } from '@/lib/render'

export default {
  name: 'description-cell',

  props: {
    editable: {
      type: Boolean,
      default: false
    },
    entry: {
      type: Object,
      default: () => ({})
    }
  },

  emits: ['description-changed'],

  data() {
    return {
      isEditing: false
    }
  },

  methods: {
    renderMarkdown,

    onClickEdit() {
      if (this.editable) {
        this.isEditing = true
        this.$nextTick(() => this.$refs.text.focus())
      }
    },

    onSaveEdit() {
      if (!this.isEditing) return
      const val = this.$refs.text.value
      this.$emit('description-changed', val)
      this.isEditing = false
    },

    onCancelEdit() {
      this.isEditing = false
    }
  }
}
</script>

<style lang="scss" scoped>
.description-cell {
  cursor: text;
  position: relative;
  padding: 0.2em 0.4em;
  vertical-align: middle;
  text-align: left;
  min-width: unset;
  max-width: unset;
  width: 100%;
  display: table-cell;
}

.description-full-text {
  white-space: pre-wrap;
  word-wrap: break-word;
  width: 100%;
  text-align: left;
  display: block;
}

.description-editor {
  width: 100%;
  resize: vertical;
  font-size: 0.95em;
  line-height: 1.4em;
  padding: 0.4em;
  box-shadow: inset 0 0 3px 0 #ccc;
  border: none;
  text-align: center;
  vertical-align: middle;
  display: block;

  &:focus {
    outline: none;
  }
}
</style>
