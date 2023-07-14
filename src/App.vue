<template>
  <div id="app">
    <h1>To-Do List</h1>
    <ToDoForm 
      @todo-added="addToDo"
    />
    <h2
      id="listSummary"
      ref="listSummary"
      tabindex="-1"
    >
      {{ getSummary }}
    </h2>
    <ul 
      class="stackLarge" 
      aria-labelledby="list-summary"
    >
      <li 
        v-for="item in ToDoItems" 
        :key="item.id"
      >
        <ToDoItem 
          :id="item.id" 
          :label="item.label" 
          :done="item.done"
          @checkboxChanged="updateDoneStatus(item.id)"
          @item-edited="editToDo(item.id, $event)"
          @item-deleted="deleteToDo(item.id)"
        />
      </li>
    </ul>
  </div>
</template>





<script>
  import ToDoItem from './components/ToDoItem.vue';
  import uniqueId from "lodash.uniqueid";
  import ToDoForm from "./components/ToDoForm.vue";

  export default {

    // props are the way that we pass data from a parent component down to its child components
    props: {

    },



    name: "app",
    components: {
      ToDoItem,
      ToDoForm
    },



    // data is the private memory of each component where you can store any ariables you need. Props are how you pass this data from a parent component down to a child component. Data is the memory of each component. This is where you would store data and any other variables you want to track.
    data() {
      return {
        ToDoItems: [
          { id: uniqueId('todo'), label: "Clean the garage", done: false },
          { id: uniqueId('todo'), label: "Wash the dishes", done: false },
          { id: uniqueId('todo'), label: "Water the garden", done: false },
        ]
      };
    },


    // Will method be called in an HTML attribute above? use methods:
    // Will method be called within {{ }} tags in text in the HTML above? Use computed:
    // Whenever you want to filter or transform your data, typically you will use a computer property for that purpose. These do not take arguments.
    methods: {
      addToDo(toDoLabel) {
        this.ToDoItems.push(
          {
            id: uniqueId('todo-'), 
            label: toDoLabel,
            done: false
          }
        )
      },


      updateDoneStatus(todoItemId) {
        const toDoToUpdate = this.ToDoItems.find(item => item.id === todoItemId);
        toDoToUpdate.done = !toDoToUpdate.done;
      },


      deleteToDo(todoId) {
        const itemIndex = this.ToDoItems.findIndex(item => item.id === todoId);
        this.ToDoItems.splice(itemIndex, 1);
        this.$refs.listSummary.focus();
      },


      editToDo(toDoId, newLabel) {
        const toDoToEdit = this.ToDoItems.find(item => item.id === toDoId);
        toDoToEdit.label = newLabel;
      }
    },


    // computed properties are great for: filtering data, handle calculations
    // can also check whether a condition is true, then use v-if within the template to determine whether to display a block of code or not
    computed: {
      getSummary() {
        const numberCompletedItems = this.ToDoItems.filter((item) => 
          item.done).length;
        
        return `${numberCompletedItems} out of ${this.ToDoItems.length} items completed.`;
      }
    }
  }
</script>





<style>
/* Global styles */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btnDanger {
  color: #fff;
  background-color: #C61500;
  border-color: #bd2130;
}

.btnFilter {
  border-color: lightgrey;
}

.btnDanger:focus {
  outline-color: #c82333;
}

.btnPrimary {
  color: #fff;
  background-color: #00A423;
}

.btnGroup {
  display: flex;
  justify-content: space-between;
}

.btnGroup > * {
  flex: 1 1 auto;
}

.btnGroup > *+* {
  margin-left: 0.8rem;
}

.labelWrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="Lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="Lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visuallyHidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"]>* {
  margin-top: 0;
  margin-bottom: 0;
}

.stackSmall > *+* {
  margin-top: 1.25rem;
}

.stackLarge > *+* {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stackSmall > *+* {
    margin-top: 1.4rem;
  }

  .stackLarge > *+* {
    margin-top: 2.8rem;
  }
}

/* End global styles */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow:
    0 2px 4px 0 rgba(0, 0, 0, 0.2),
    0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}

#app > form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}</style>

