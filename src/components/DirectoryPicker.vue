<template>
  <div class="directory-picker" role="input">
    <ul class="directory-picker__root">
      <directory-picker-item
          v-for="item in localData"
          :key="item.id"
          v-bind="item"
      />
    </ul>
    <footer>
      <button @click="handleApplyClick">Подтвердить</button>
      <button @click="handleCancelClick">Отмена</button>
    </footer>
  </div>
</template>

<script>

import DirectoryPickerItem from '@/components/DirectoryPickerItem'

export default {
  name: 'DirectoryPicker',
  components: {DirectoryPickerItem},
  provide() {
    return {
      selectItem: this.selectItem,
      expandItem: this.expandItem,
    }
  },
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    value: {
      type: Object,
    },
  },
  data() {
    return {
      pickedValue: null,
      expandedItems: [],
    }
  },
  computed: {
    localData() {
      const callback = (acc, item) => {
        item.children = item.children.reduce(callback, [])
        item.isActive = item.id === this.pickedValue?.id
        item.isExpanded = this.expandedItems.includes(item.id)
        return [...acc, item]
      }

      return this.data.reduce(callback, [])
    },
  },
  methods: {
    handleCancelClick() {
      this.$emit('cancel')
    },

    handleApplyClick() {
      this.$emit('select', this.pickedValue)
    },

    selectItem(item) {
      this.pickedValue = item
    },

    expandItem(id) {
      if (this.expandedItems.includes(id)) {
        this.expandedItems = this.expandedItems.filter(_id => _id !== id)
      } else {
        this.expandedItems.push(id)
      }
    },
  },
  created() {
    this.pickedValue = this.value

    const callback = (acc, item) => {
      if (!acc && item.id === this.value?.id) return []
      if (!acc && item.children) {
        const result = item.children.reduce(callback, null)
        return result ? [...result, item.id] : null
      }

      return acc
    }

    this.expandedItems = this.data.reduce(callback, null) || []
    console.log('this.expandedItems', this.expandedItems)
  },
}
</script>

<style scoped>
.directory-picker {
  display: grid;
  grid-template-rows: 1fr 40px;
  min-width: 400px;
  padding-left: 1em;
}

.directory-picker__root {
  max-height: 500px;
  min-height: 300px;
  overflow: auto;
  padding-left: 1em;
}

.directory-picker footer {
  display: flex;
  justify-content: flex-end;
  width: 100%;
}

.directory-picker button:not(:first-child) {
  margin-left: 1em;
}
</style>