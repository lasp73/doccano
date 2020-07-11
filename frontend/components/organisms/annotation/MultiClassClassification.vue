<template>
  <v-combobox
    :value="annotatedLabels"
    :items="sortedLabels"
    @input="add"
    item-text="text"
    label="Label"
    hide-selected
    chips
    multiple
  >
    <template v-slot:selection="{ attrs, item, select, selected }">
      <v-chip
        :active="isValidLabel(item)"
        v-bind="attrs"
        :input-value="selected"
        :color="backgroundColor(item)"
        :text-color="textColor(item)"
        @click="select"
        @click:close="remove(item.id)"
        close
      >
        {{ item.text }}
      </v-chip>
    </template>
  </v-combobox>
</template>

<script>
import { idealColor } from '~/plugins/utils'

export default {
  props: {
    labels: {
      type: Array,
      default: () => [],
      required: true
    },
    annotations: {
      type: Array,
      default: () => ([]),
      required: true
    },
    addLabel: {
      type: Function,
      default: () => ([]),
      required: true
    },
    deleteLabel: {
      type: Function,
      default: () => ([]),
      required: true
    }
  },

  computed: {
    sortedLabels() {
      return this.labels.slice().sort((a, b) => ((a.text < b.text) ? -1 : 1))
    },
    annotatedLabels() {
      const labelIds = this.annotations.map(item => item.label)
      return this.labels.filter(item => labelIds.includes(item.id))
    },
    labelObject() {
      const obj = {}
      for (const label of this.labels) {
        obj[label.id] = label
      }
      return obj
    }
  },

  methods: {
    isValidLabel(item) {
      return typeof item === 'object'
    },
    textColor(item) {
      if (typeof item === 'object') {
        return idealColor(item.background_color)
      } else {
        return ''
      }
    },
    backgroundColor(item) {
      if (typeof item === 'object') {
        return item.background_color
      } else {
        return ''
      }
    },
    add(labels) {
      const label = labels[labels.length - 1]
      if (typeof label === 'object') {
        this.addLabel(label.id)
      }
    },
    remove(labelId) {
      const annotation = this.annotations.find(item => item.label === labelId)
      this.deleteLabel(annotation.id)
    }
  }
}
</script>
