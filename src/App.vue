<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div class="container">
      <div class="columns is-multiline">
        <div class="column is-6 is-offset-3">
          <div class="card" v-bind:style="{ 'background-color': colorHex }">
            <div class="card-content">
              <form @submit.prevent="addTask">
                <div class="field">
                  <div class="control">
                    <input class="input" type="text" placeholder="Judul" v-model="title">
                  </div>
                </div>
                <div class="field">
                  <div class="control">
                    <textarea rows="1" class="textarea" placeholder="Deskripsi" v-model="description"></textarea>
                  </div>
                </div>
                <nav class="level">
                  <!-- Left side -->
                  <div class="level-left">
                    <div class="level-item">
                      <div class="select">
                        <select v-model="color">
                          <option v-bind:value="null" disabled selected>Warna</option>
                          <option value="red">Merah</option>
                          <option value="yellow">Kuning</option>
                          <option value="green">Hijau</option>
                          <option value="blue">Biru</option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <!-- Right side -->
                  <div class="level-right">
                    <div class="level-item">
                       <div class="field">
                          <div class="control">
                            <button type="submit" class="button is-primary">Tambahkan</button>
                          </div>
                        </div>
                    </div>
                  </div>
                </nav>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div class="columns is-multiline">
        <div class="column">
          <p class="is-size-4">
            To Do
          </p>
          <template v-if="filterToDo && filterToDo.length">
            <Card
              v-for="todo in filterToDo"
              :key="todo.id"
              :title="todo.title"
              :description="todo.description"
              :color="todo.color"
              :card-id="todo.id"
              :status="todo.status"
            />   
          </template>
          <template v-else>
            <p>Kosong</p>
          </template>
        </div>
        <div class="column">
          <p class="is-size-4">
            Doing
          </p>
          <template v-if="filterDoing && filterDoing.length">
            <Card
              v-for="doing in filterDoing"
              :key="doing.id"
              :title="doing.title"
              :description="doing.description"
              :color="doing.color"
              :card-id="doing.id"
              :status="doing.status"
            />    
          </template>
          <template v-else>
            <p>Kosong</p>
          </template>     
        </div>
        <div class="column">
          <p class="is-size-4">
            Done
          </p>
          <template v-if="filterDone && filterDone.length">
            <Card
              v-for="done in filterDone"
              :key="done.id"
              :title="done.title"
              :description="done.description"
              :color="done.color"
              :card-id="done.id"
              :status="done.status"
            />  
          </template>
          <template v-else>
            <p>Kosong</p>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Card from './components/Card.vue'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    // HelloWorld
    Card
  },
  data() {
    return {
      title: null,
      description: null,
      tasks: [],
      color: null
    }
  },
  computed: {
    colorHex () {
      if (this.color === 'yellow') {
          return '#fff475'
      } else if (this.color === 'red') {
          return '#f28b82'
      } else if (this.color === 'blue') {
          return '#a3dcf0'
      } else if (this.color === 'green') {
          return '#ccff90'
      } else {
          return '#ffffff'
      }
    },
    filterToDo() {
      return this.tasks.filter(todo => {
        return todo.status === 'To Do' 
      })
    },
    filterDoing() {
      return this.tasks.filter(todo => {
        return todo.status === 'Doing' 
      })
    },
    filterDone() {
      return this.tasks.filter(todo => {
        return todo.status === 'Done' 
      })
    }
  },
  created() {
    this.getTasks()
  },
  methods: {
    getTasks () {
      axios({
        method: 'GET',
        url: 'http://localhost:3000/tasks',
        headers: {
          'Content-Type': 'application/json',
          'Access-Control-Allow-Origin': '*'
        }
      })
      .then(response => {
        this.tasks = response.data.values
      })
      .catch(error => {
        // eslint-disable-next-line
        console.log(error)
      })
    },
    addTask () {
     axios({
        method: 'POST',
        url: 'http://localhost:3000/tasks',
        headers: {
          'Content-Type': 'application/json',
          'Access-Control-Allow-Origin': '*'
        },
        data: {
          title: this.title,
          description: this.description,
          status: 'To Do',
          color: this.color
        }
      })
      .then(response => {
        // eslint-disable-next-line
        console.log(response)
        this.title = null
        this.description = null
        this.color = null
        this.getTasks()
      })
      .catch(error => {
        // eslint-disable-next-line
        console.log(error)
        this.title = null
        this.description = null
        this.color = null
      })
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma/bulma.sass';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
