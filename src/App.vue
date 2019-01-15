<template>
  <div id="app">
    <Sondage/>
  </div>
</template>

<script>
import Sondage from './components/Sondage'

export default {
  name: 'App',
  components: {
    Sondage
  }
}
</script>

<style>
	body {
		background-image: url('./assets/pipes.png');
	}
</style>
