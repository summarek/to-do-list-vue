<template>
  <div>
    <div class="holder">
      <form @submit.prevent="addTask">
        <input
          type="text"
          placeholder="What do you have to do?"
          v-model="task"
          v-validate="'min:5'"
          name="task"
        >
        <transition
          name="alert-in"
          enter-active-class="animated flipInX"
          leave-active-class="animated flipOutX"
        >
          <p class="alert" v-if="errors.has('task')">{{errors.first('task')}}</p>
        </transition>
      </form>
      <ul>
        <transition-group
          name="list"
          enter-active-class="animated bounceInUp"
          leave-active-class="animated bounceOutDown"
        >
          <li v-for="(data, index) in tasks" :key="index">
            {{data.task}}
            <span id="xMark" v-on:click="removeTask(index)">DONE</span>
          </li>
        </transition-group>
      </ul>
      <p>Task list.</p>
    </div>

    <div class="holder">
      <ul>
        <transition-group
          name="list"
          enter-active-class="animated bounceInUp"
          leave-active-class="animated bounceOutDown"
        >
          <li v-for="(data, index) in doneTasks" :key="index">
            {{doneTasks[index]}}
            <span id="xMark" v-on:click="removeDoneTask(index)">X</span>
          </li>
        </transition-group>
      </ul>
      <p>YOU DID IT!</p>
    </div>
  </div>
</template>

<script>
import { debuglog } from "util";
export default {
  name: "PostExample",
  data() {
    return {
      checked: false,
      task: "",
      tasks: [],
      doneTask: "",
      doneTasks: []
    };
  },
  mounted() {
    if (localStorage.getItem("tasks")) {
      try {
        this.tasks = JSON.parse(localStorage.getItem("tasks"));
      } catch (e) {
        localStorage.removeItem("tasks");
      }
    }
    if (localStorage.getItem("doneTasks")) {
      try {
        this.doneTasks = JSON.parse(localStorage.getItem("doneTasks"));
      } catch (e) {
        localStorage.removeItem("doneTasks");
      }
    }
  },
  methods: {
    addTask() {
      this.$validator.validateAll().then(result => {
        if (result && this.task !== "") {
          this.tasks.push({ task: this.task });
          this.task = "";
          this.saveTasks();
        } else {
          console.log("not valid");
        }
      });
    },

    removeTask(id) {
      this.doneTasks.push(this.tasks[id].task);
      this.tasks.splice(id, 1);
      this.saveTasks();
      this.saveDoneTasks();
    },

    removeDoneTask(id) {
      this.doneTasks.splice(id, 1);
      this.saveDoneTasks();
      this.saveTasks();
    },
    saveTasks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    },
    saveDoneTasks() {
      const parsed = JSON.stringify(this.doneTasks);
      localStorage.setItem("doneTasks", parsed);
    }
  }
};
</script>

<style scoped>
@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
.holder {
  background: #fff;
}

ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

ul li {
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  border-left: 5px solid #3eb3f6;
  margin-bottom: 2px;
  color: #3e5252;
}

p {
  text-align: center;
  padding: 30px 0;
  color: gray;
}

.container {
  box-shadow: 0px 0px 40px lightgray;
}

input {
  text-align: center;
  width: calc(100% - 40px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  background-color: #323333;
  color: rgb(131, 131, 131);
}
.alert {
  background: #fdf2ce;
  font-weight: bold;
  display: inline-block;
  padding: 5px;
  margin-top: -20px;
}

.alert-in-enter-active {
  animation: bounce-in 0.5s;
}
.alert-in-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
#xMark {
  float: right;
  font-weight: bold;
  cursor: pointer;
  padding: 5px;
  color: white;
  border-radius: 5px;
  background-image: linear-gradient(120deg, #84fab0 0%, #8fd3f4 100%);
}
</style>
