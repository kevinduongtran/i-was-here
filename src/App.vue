<template>
  <div id="app" class="container-fluid p-3 bg-info">
    <div class="row-fluid rounded">
      <div class="fixed bg-dark fill rounded-left">
        <SideBar />
      </div>
      <div class="bg-light fill rounded">
        <Map/>
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import Map from "./components/Map.vue";

export default {
  name: "App",
  components: {
    SideBar,
    Map,
  },
};
</script>

<style>
html,
body {
  height: 100%;
}
.container-fluid {
  height: 100%;
  overflow-y: hidden; /* don't show content that exceeds my height */
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: calc(100%- 24px);
}
.fixed {
  width: 250px;
  float: left;
}
.fill {
  height: calc(100vh - 32px);
}
</style>
