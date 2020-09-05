<template>
  <div id="app">
    <Header />
    <Convert />
  </div>
</template>

<script>

// @ is an alias to /src
import Convert from '../components/Convert';

export default {
  name: 'Home',
  components: {
    Convert
  }
}
</script>

<style scoped>
  
</style>