<template>
  <div ref="modal" class="overlay" :hidden="!open" @click="open = false" @keydown.esc="open = false">
    <div role="dialog" :aria-label="heading" aria-modal="true" class="fighters-list">
      <h2 class="dialog-heading">{{ heading }}</h2>
      <hr>
      <div class="dialog-body">
        <slot />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["heading", "open"],
  data() {
  },
};
</script>

<style>
.overlay {
  position: fixed;
  overflow-y: auto;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;
}

/* [role="dialog"] {
  box-sizing: border-box;
  padding: 15px;
  border: 1px solid #000;
  background-color: #fff;
  min-height: 100vh;
} */

[role="dialog"] {
  box-sizing: border-box;
  padding: 15px;
  border: 2px solid black;
  border-radius: 10px;
  box-shadow: 4px 4px 8px;
  background-color: #fff;
  width: fit-content;
  margin: auto;
  margin-top: 2rem;
  margin-bottom: 2rem;
}


</style>
