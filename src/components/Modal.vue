<template >
  <div class="modal-backdrop" v-if="show">
    <div class="modal-container">
      <h1>{{ modalText }}</h1>
      <div class="btn-container">
        <template>
          <button class="btn btn-rounded btn-teal" @click="confirmHandler">Confirm</button>
          <button class="btn btn-rounded btn-red" @click="cancelHandler">Cancel</button>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  data: function() {
    return {
      title: "this is modal"
    };
  },
  props: {
    modalText: String,
    confirmHandler: Function,
    cancelHandler: Function,
    show: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-backdrop {
  top: 0;
  left: 0;
  width: 100%;
  position: absolute;
  height: 100vh;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 5;
}
.modal-container {
  padding: 1rem;
  /* color: #42b983; */
  background: #fff;
  border-radius: 0.4rem;
  width: 80%;
  max-width: 760px;
  margin: 0 auto;
  z-index: 10;
}
</style>
