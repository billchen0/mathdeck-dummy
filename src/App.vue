<template>
  <img alt="Vue logo" src="./assets/logo.png">

  <h3 v-if="chip" draggable="true" @dragover.stop @dragstart="startDrag($event)">{{ this.eqn }}</h3>
  <h1 v-if="msg !== ''">{{ this.msg }}</h1>
  
  <div class="separator"></div>

  <div>
    <form>
      <input v-model="query" type="text" requried @submit.prevent="onFormSubmit">
      <button @click="postMessage">Search</button>
      
    </form>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      eqn: '',
      chip: false,
      msg: '',

      query: '',
    }
  },
  methods: {
    receiveMsg(event) {
      if (event.origin === "http://localhost:3000") {
          let data = JSON.parse(event.data)
          if (data['type'] == 'chip') {
            this.chip = true
            this.eqn = data['string']
          } else if (data['type'] == 'message') {
            this.msg = data['string']
          }
          
      } else {
        this.msg = ''
      }
    },
      postMessage() {
        parent.postMessage(this.query, "*")
      },
      startDrag(event) {
        const data = {
                target: 'mathdeck',
                type: 'chip',
                string: '2x+3'
            }
            event.dataTransfer.setData('text/plain', JSON.stringify(data))
      }
  },
  mounted() {
    window.addEventListener('message', this.receiveMsg, false)
  }
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

h3 {
  display: table;
  margin: 0px auto 0px auto;
  padding: 5px;
  background-color: grey;
  color: #ffff
}

.separator {
  height: 50px;
}
</style>
