<template>
  <li :class="classes">
    <div
        v-if="children"
        class="directory-picker-item__collapse-area"
        @click="handleExpand"
    />
    <span
        class="directory-picker-item__title"
        @click.prevent="handleSelect"
    >
      {{ title }}
    </span>
    <ul v-if="isExpanded">
      <directory-picker-item
          v-for="item in children"
          :key="item.id"
          v-bind="item"
      />
    </ul>
  </li>
</template>

<script>
export default {
  name: 'DirectoryPickerItem',
  inject: ['selectItem', 'expandItem'],
  props: {
    id: {
      type: Number,
      required: true,
    },
    title: {
      type: String,
      default: '-',
    },
    isActive: {
      type: Boolean,
      default: false,
    },
    isExpanded: {
      type: Boolean,
      default: false,
    },
    children: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    classes() {
      return {
        'directory-picker-item': true,
        'directory-picker-item--expanded': this.isExpanded && this.children.length,
        'directory-picker-item--active': this.isActive,
        'directory-picker-item--no-child': !this.children.length,
      }
    }
  },
  methods: {
    handleExpand() {
      this.expandItem(this.id)
    },

    handleSelect() {
      this.selectItem(this.$props)
    },
  },
}
</script>

<style scoped>
.directory-picker-item {

}

.directory-picker-item__title {
  cursor: pointer;
  padding-left: .5em;
  white-space: nowrap;
}

.directory-picker-item--active > .directory-picker-item__title {
  font-weight: 700;
}

.directory-picker-item::marker {
  content: '▼';
  font-size: .7em;
}

.directory-picker-item--expanded::marker {
  content: '▲';
}

.directory-picker-item--no-child::marker {
  content: '';
}

.directory-picker-item__collapse-area {
  position: absolute;
  left: -1em;
  width: 1em;
  height: 1em;
  cursor: pointer;
}

</style>