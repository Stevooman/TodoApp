<template>
  <form @submit.prevent="onSubmit">
    <h2 class="labelWrapper">
      <label 
        for="new-todo-input"
        class="labelLg"
      >
        What needs to be done?
      </label>
    </h2>
    <input 
      type="text" 
      id="new-todo-input" 
      name="new-todo" 
      autocomplete="off" 
      v-model.trim.lazy="label"
      class="inputLg"
    />
    <button 
      type="submit"
      class="btn btnPrimary btnLg"
    >Add to-do
    </button>
  </form>
</template>




<script>
  export default {


    // methods holds any methods we might need to call within this component
    methods: {
      onSubmit() {
        if (this.label === '') {
          return;
        }
        this.$emit('todo-added', this.label);
        this.label = '';
      }
    },

    

    data() {
      return {
        label: ""
      }
    }
  }
</script>





<style></style>