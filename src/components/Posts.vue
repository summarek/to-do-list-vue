<template>
  <div>
    <button @click="resetAll" class="btn-shine">
      <span>RESET</span>
    </button>
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
          <li class="date" v-for="(data, index) in doneTasks" :key="index">
            <small>{{dateTime[index]}}</small>
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
      dateTime: [],
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

    if (localStorage.getItem("dateTime")) {
      try {
        this.dateTime = JSON.parse(localStorage.getItem("dateTime"));
      } catch (e) {
        localStorage.removeItem("dateTime");
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
      var m = moment().format("DD/MM/YYYY HH:mm");
      console.log(m);
      this.dateTime.push(m);
      this.saveDate();
      this.doneTasks.push(this.tasks[id].task);
      this.tasks.splice(id, 1);
      this.saveTasks();
      this.saveDoneTasks();
    },

    removeDoneTask(id) {
      this.doneTasks.splice(id, 1);
      this.dateTime.splice(id, 1);
      this.saveDate();
      this.saveDoneTasks();
      this.saveTasks();
    },
    saveTasks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    },
    saveDate() {
      const parsed = JSON.stringify(this.dateTime);
      localStorage.setItem("dateTime", parsed);
    },
    saveDoneTasks() {
      const parsed = JSON.stringify(this.doneTasks);
      localStorage.setItem("doneTasks", parsed);
    },
    resetAll() {
      this.dateTime = [];
      this.task = "";
      this.tasks = [];
      this.doneTask = "";
      this.doneTasks = [];
      this.saveDate();
      this.saveDoneTasks();
      this.saveTasks();
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
  padding: 25px;
  line-break: auto;
  word-break: break-all;
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
<style lang="scss" scoped>
button {
  position: relative;
  margin: 0;
  padding: 5px 12px;
  height: 60px;
  width: 150px;
  outline: none;
  text-decoration: none;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  text-transform: uppercase;
  background-color: #ffffff;
  border: 1px solid rgba(22, 76, 167, 0.6);
  border-radius: 10px;
  color: #1d89ff;
  font-weight: 400;
  font-size: 20px;
  font-family: inherit;
  z-index: 0;
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.02, 0.01, 0.47, 1);
  span {
    color: #164ca7;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 0.7px;
  }
  &:hover {
    animation: rotate 0.7s ease-in-out both;
    span {
      animation: storm 0.7s ease-in-out both;
      animation-delay: 0.06s;
    }
  }
}

@keyframes rotate {
  0% {
    transform: rotate(0deg) translate3d(0, 0, 0);
  }
  25% {
    transform: rotate(3deg) translate3d(0, 0, 0);
  }
  50% {
    transform: rotate(-3deg) translate3d(0, 0, 0);
  }
  75% {
    transform: rotate(1deg) translate3d(0, 0, 0);
  }
  100% {
    transform: rotate(0deg) translate3d(0, 0, 0);
  }
}
@keyframes storm {
  0% {
    transform: translate3d(0, 0, 0) translateZ(0);
  }
  25% {
    transform: translate3d(4px, 0, 0) translateZ(0);
  }
  50% {
    transform: translate3d(-3px, 0, 0) translateZ(0);
  }
  75% {
    transform: translate3d(2px, 0, 0) translateZ(0);
  }
  100% {
    transform: translate3d(0, 0, 0) translateZ(0);
  }
}
.btn-shine {
  top: -10px;
  border: 1px solid;
  overflow: hidden;
  position: relative;
  span {
    z-index: 20;
    font-size: 1.5em;
  }
  &:after {
    background: #38ef7d;
    content: "";
    height: 155px;
    left: -75px;
    opacity: 0.4;
    position: absolute;
    top: -50px;
    transform: rotate(35deg);
    transition: all 550ms cubic-bezier(0.19, 1, 0.22, 1);
    width: 50px;
    z-index: -10;
  }
  &:hover {
    &:after {
      left: 120%;
      transition: all 550ms cubic-bezier(0.19, 1, 0.22, 1);
    }
  }
}
</style>
