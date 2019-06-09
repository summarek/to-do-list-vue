<template>
  <div id="app">
    <h1>TO DO LIST</h1>
    <PostExample></PostExample>
  </div>
</template>

<script>
import PostExample from "./components/Posts.vue";

export default {
  name: "app",
  components: {
    PostExample
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700");

body {
  background-color: #eeeeee;
  font-family: "Montserrat", sans-serif;
  display: grid;
  grid-template-rows: auto;
  justify-items: center;
  align-items: center;
}
body,
html {
  margin: 0;
  height: 100%;
}
#app {
  width: 70%;
}
h1 {
  text-align: center;
}
@media only screen and (max-width: 500px) {
  #app {
    width: 100%;
  }
  small {
    position: absolute;
    bottom: 0;
    left: 0;
    padding: 6px;
    padding-left: 26px;

    color: rgba(0, 0, 0, 0.432);
  }
  ul li {
    position: relative;
  }
}
@media only screen and (max-width: 768px) {
  #app {
    width: 90%;
  }
}

@media only screen and (max-width: 1268px) {
  #app {
    width: 80%;
  }
}
</style>
