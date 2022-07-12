<template>
  <div v-if="isOpen" class="modal" @click="handleClick">
    <div class="modal__content">
      <header class="modal__header">
        <h3 class="modal__title">{{ title }}</h3>
      </header>
      <div class="modal__wrapper">
        <slot/>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'DialogWindow',
  props: {
    isOpen: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: '-',
    },
  },
  methods: {
    handleToggle() {
      this.$emit('toggle', false)
    },

    handleClick({target}) {
      target === this.$el && this.handleToggle(false)
    },
  },
}
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  display: grid;
  place-content: center;
  background-color: #999999ee;
  overflow: auto;
}

.modal__content {
  background-color: #ffffff;
  box-shadow: 0 0 10px 0 #333333;
  padding: 1rem 2rem;

  grid-template-columns: 1fr;
  grid-template-rows: 1em 1fr min-content;
}

.modal__header {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.modal__wrapper {
  min-height: 300px;
}

</style>