<template>
  <div id="app">
    <v-app>
    <game />
    </v-app>
  </div>
</template>

<script>
import Game from './components/Game'

export default {
  name: 'app',
  components: { Game },
  data () {
    return {
      msg: 'debuging now'
    }
  }
}
</script>
