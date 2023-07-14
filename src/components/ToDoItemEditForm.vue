<template>
  <form class="stackSmall" @submit.prevent="onSubmit">
    <div>
      <label 
        class="editLabel"
      >
      Edit Name for &quot;{{ label }}&quot;
      </label>
      <input 
        ref="labelInput"
        :id="id" 
        type="text" 
        autocomplete="off" 
        v-model.lazy.trim="newLabel" 
      />
    </div>
    <div class="btnGroup">
      <button 
        type="button" 
        class="btn" 
        @click="onCancel"
      >
        Cancel
      </button>
      <button 
        type="submit" 
        class="btn btnPrimary"
      >
        Save
      </button>
    </div>
  </form>
</template>





<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },

  // Runs after your component is mounted to the DOM. Can access refs here.
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },

  data() {
    return {
      newLabel: this.label,
    };
  },

  methods: {
    onSubmit() {
      if (this.newLabel && this.newLabel !== this.label) {
        this.$emit("item-edited", this.newLabel);
      }
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
};
</script>





<style scoped>
.edit-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}

input {
  display: inline-block;
  margin-top: 0.4rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}

form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

form > * {
  flex: 0 0 100%;
}
</style>
