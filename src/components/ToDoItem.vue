<template>
  <div 
    class="stackSmall"
    v-if="!isEditing"
  >
    <div class="customCheckbox">
      <input 
        type="checkbox" 
        :id="id" 
        :checked="isDone"
        class="checkbox"
        @change="$emit('checkboxChanged')"
      />
      <label 
        :for="id"
        class="checkboxLabel"
      >
        {{ label }}
      </label>
    </div>
    <div class="btnGroup">
      <button 
        type="button" 
        class="btn"
        ref="editButton" 
        @click="toggleToItemEditForm"
      >
        Edit
      </button>
      <button 
        type="button" 
        class="btn btnDanger" 
        @click="deleteToDo"
      >
        Delete
      </button>
    </div>
  </div>
  <ToDoItemEditForm 
    v-else 
    :id="id" 
    :label="label" 
    @item-edited="itemEdited"
    @edit-cancelled="editCancelled"
  />
</template>





<script>
  import ToDoItemEditForm from './ToDoItemEditForm.vue';

  export default {

    props: {
        id: { required: true, type: String },
        label: { required: true, type: String },
        done: { default: false, type: Boolean }
    },


    data() {
        return {
            isEditing: false
        };
    },


    computed: {
      isDone() {
        return this.done;
      }
    },


    methods: {
        deleteToDo() {
            this.$emit("item-deleted");
        },

        toggleToItemEditForm() {
            this.isEditing = true;
        },

        itemEdited(newLabel) {
          this.$emit("item-edited", newLabel);
          this.isEditing = false;
          this.focusEditButton();
        },

        editCancelled() {
          this.isEditing = false;
          this.focusEditButton();
        },

        focusEditButton() {
          // $nextTick allows you to execute code after you have changed some data and Vue has updated the virtual DOM based on your data change, but before the browser has rendered that change on the page. Essentially, this lets us bring the focus back to the edit button after saving an edit (whereas before, after saving, the focus would be off the edit button - nowhere).
          // $nextTick says: "Run the code in this callback function before rendering contents in the browser"
          this.$nextTick(() => {
            const editButtonRef = this.$refs.editButton;
            editButtonRef.focus();
          });
        }
    },

    components: { 
      ToDoItemEditForm 
    }
}
</script>





<style scoped>
  .customCheckbox > .checkboxLabel {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
.customCheckbox > .checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  box-sizing: border-box;
  width: 100%;
  height: 40px;
  height: 2.5rem;
  margin-top: 0;
  padding: 5px;
  border: 2px solid #0b0c0c;
  border-radius: 0;
  appearance: none;
}
.customCheckbox > input:focus {
  outline: 3px dashed #fd0;
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px;
}
.customCheckbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.25;
  display: block;
  position: relative;
  min-height: 40px;
  margin-bottom: 10px;
  padding-left: 40px;
  clear: left;
}
.customCheckbox > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  position: absolute;
  z-index: 1;
  top: -2px;
  left: -2px;
  width: 44px;
  height: 44px;
  margin: 0;
  opacity: 0;
}
.customCheckbox > .checkboxLabel {
  font-size: inherit;
  font-family: inherit;
  line-height: inherit;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}
.customCheckbox > label::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 40px;
  border: 2px solid currentcolor;
  background: transparent;
}
.customCheckbox > input[type="checkbox"]:focus + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.customCheckbox > label::after {
  box-sizing: content-box;
  content: "";
  position: absolute;
  top: 11px;
  left: 9px;
  width: 18px;
  height: 7px;
  transform: rotate(-45deg);
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  opacity: 0;
  background: transparent;
}
.customCheckbox > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}

.btnDanger {
  background-color: #BF220F;
}

@media only screen and (min-width: 40rem) {
  label,
  input,
  .custom-checkbox {
    font-size: 19px;
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}

</style>
