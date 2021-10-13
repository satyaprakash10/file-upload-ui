<template>
  <div>
    <!-- header  -->
    <TheNavbar />

    <!-- Main content -->
    <div class="w-full relative">
      <FileManagingDemo />
    </div>
  </div>
</template>

<script>
import TheNavbar from './components/TheNavbar.vue'
import FileManagingDemo from './components/FileManagingDemo.vue'
export default {
  name: 'App',
  components: {
    TheNavbar,
    FileManagingDemo,
  },

  data() {
    return {}
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
