<template>
  <main>
    <dialog-window
        v-bind="dialog"
        @toggle="handleDialogToggle"
    >
      <directory-picker
          :data="picker.data"
          :value="picker.value"
          @select="handleSelect"
          @cancel="handleDialogToggle(false)"
      />
    </dialog-window>
    <button @click="handleDialogToggle(true)">Открыть</button>
    <div>
      <h3>Выбранная папка:</h3>
      <output>{{ selectedFolder }}</output>
    </div>
  </main>
</template>

<script>

import DialogWindow from '@/components/DialogWindow'
import DirectoryPicker from '@/components/DirectoryPicker'

export default {
  name: 'App',
  components: {DirectoryPicker, DialogWindow},
  data() {
    return {
      dialog: {
        isOpen: false,
        title: 'Выберите директорию',
      },
      picker: {
        data: [],
        value: null,
      },
    }
  },
  methods: {
    handleDialogToggle(value) {
      this.dialog.isOpen = value
    },

    handleSelect(item) {
      this.picker.value = item

      this.handleDialogToggle(false)
    },

    generateData({depth, width} = {depth: 3, width: 5}) {
      return new Array(width)
          .fill({})
          .map((item, index) => ({
            id: Math.floor(Math.random() * 1000000000),
            title: `Folder - ${depth}-${index}`,
            children: depth
                ? this.generateData({width, depth: depth - 1})
                : [],
          }))
    },
  },
  computed: {
    selectedFolder() {
      return this.picker.value?.title ?? '-'
    },
  },
  created() {
    this.picker.data = this.generateData()
    document.title = 'Тестовое задание'
  },
}
</script>

<style>
* {
  box-sizing: border-box;
  position: relative;
}

body {
  margin: 0;
  min-height: 100vh;
}

button {
  cursor: pointer;
}

main {
  display: grid;
  place-content: center;
  min-height: 100vh;
}
</style>
