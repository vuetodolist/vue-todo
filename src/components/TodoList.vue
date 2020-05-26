<template>
  <div class="todo-list">
    <!-- Loads when the task list is empty -->
    <div v-if="!todoLists.length" class="empty-list">
      <i class="fa fa-calendar-check-o"></i>
      <h1>Add your first todo</h1>
      <h3>What do you want to get done today ?</h3>
    </div>
    <!-- Loades when task list available -->
    <div v-else>
      <ul>
        <li v-for="todo in todoLists" :key="todo.id">
          <div class="list-check-container">
            <!-- Task list mode -->
            <div
              class="list-check"
              v-bind:class="{ lineThrough: todo.isCompleted }"
              v-if="todo.edit"
            >
              <input
                type="checkbox"
                id="checkbox"
                v-model="todo.isCompleted"
                @click="taskCompleted(todo)"
                class="task-checkbox checkbox-primary"
              />
              <span @click="handleEdit(todo)">{{todo.task}}</span>
            </div>
            <!-- Task edit mode (onclicking on top of the item list) -->
            <div v-else class="list-check-update">
              <input
                :ref="todo.id"
                type="text"
                v-focus
                @blur="updateTask(todo)"
                v-model="todo.task"
                @keyup.enter="updateTask(todo)"
              />
            </div>
            <!-- Deleted span -->
            <span @click="deleteTask(todo.id)" class="delete-list">x</span>
          </div>
        </li>
      </ul>
    </div>
    <!-- Add new input container. New task is added, when the input filed is out of focus or blur.
    And when user hit enter. No button action integrated-->
    <span class="add-new-input">
      <input
        type="text"
        v-model="task"
        @blur="addTask()"
        @keyup.enter="addTask()"
        :placeholder="placeHolder"
        @focus="placeHolder=''"
      />
    </span>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  props: {
    todoLists: Array
  },
  data: function() {
    return {
      task: "",
      placeHolder: "+ List Item" // Initial place holder 
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    // Event triggers from child to parent for passing data
    addTask: function() {
      this.placeHolder = "+ List Item";
      if (this.task) {
        this.$emit("clicked", this.task);
        this.task = "";
      }
    },
    deleteTask: function(id) {
      this.$emit("delete-task", id);
    },
    taskCompleted: function(id) {
      this.$emit("completed-task", id);
    },
    handleEdit: function(todo) {
      this.$emit("edit-task", todo);
    },
    updateTask: function(todo) {
      this.$emit("update-task", todo);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.lineThrough {
  text-decoration: line-through;
}
ul {
  list-style-type: none;
  padding-left: 10px !important;
  margin-top: 0;
  margin-bottom: 20px;
}
.list-check {
  font-size: 2em;
  font-weight: 500;
}
.task-checkbox {
  transform: scale(2);
  margin-right: 30px;
  vertical-align: middle;
  margin-bottom: 5px;
}
.list-check-container {
  display: flex;
  justify-content: space-between;
}
.delete-list {
  transform: scale(1.5);
  margin-right: 2.5vw;
  cursor: pointer;
  font-size: 2em;
  font-weight: 600;
}
.add-new-input input,
.list-check-update input {
  border-right: none;
  border-left: none;
  border-top: 10px solid #2d3e50;
  border-bottom: 10px solid #2d3e50;
  border-width: 100%;
  width: 100%;
  height: 50px;
  font-size: 19px;
  margin-top: 15px;
  font-size: 1.5em;
  /* font-weight: 700; */
  outline: none;
  border-radius: 0;
}
.list-check-update input {
  padding-top: 0px;
  padding-bottom: 15px;
  padding-left: 90px;
  font-size: 2em;
  color: #2c3e50;
  border-top: none;
  border-bottom: none;
  margin-top: 0;
  transform: scale(1.5);
}
.todo-list {
  margin-top: 30px;
}
li {
  margin-bottom: 15px;
}
.empty-list {
  text-align: center;
  margin: 4em 0;
}
.fa-calendar-check-o {
  font-size: 8em;
}
/* Media query for screen less than 700px */
@media only screen and (max-device-width: 700px) and (-webkit-min-device-pixel-ratio: 2) {
  .list-check {
    font-size: 6vw;
  }
  .delete-list {
    font-size: 6vw;
  }
  .task-checkbox {
    height: 3vw;
    margin-right: 10px;
  }
  .addNewInput input {
    font-size: 6vw;
  }
  .fa-calendar-check-o {
  font-size: 30vw;
}
.empty-list h1 {
  font-size: 9vw;
}
.empty-list h3 {
  font-size: 5vw;
}
.list-check-update input {
  font-size: 5vw;
  padding-left: 50px;
  padding-bottom: 0px;
}
}
</style>
