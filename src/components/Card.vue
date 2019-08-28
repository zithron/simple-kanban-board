<template>
    <div class="card" v-bind:style="{'background-color': colorHex}">
        <div class="card-content">
            <h1 class="is-size-5">
                {{ title }}
            </h1>
            <p>
                {{ description }}
            </p>
            <div class="dropdown" :class="{'is-active': isActive}" @click="toggleDropdown">
                <div class="dropdown-trigger">
                    <button class="button" aria-haspopup="true" aria-controls="dropdown-menu">
                    <span>Status</span>
                    <span class="icon is-small">
                        <i class="fa fa-angle-down" aria-hidden="true"></i>
                    </span>
                    </button>
                </div>
                <div class="dropdown-menu" id="dropdown-menu" role="menu">
                    <div class="dropdown-content">
                        <a @click="changeStatus(cardId, 'To Do')" class="dropdown-item">
                            To Do
                        </a>
                        <a @click="changeStatus(cardId, 'Doing')" class="dropdown-item">
                            Doing
                        </a>
                        <a @click="changeStatus(cardId, 'Done')" class="dropdown-item">
                            Done
                        </a>
                    </div>
                </div>
            </div>
            <div class="dropdown" :class="{'is-active': isActivatedChangeColor}" @click="toggleDropdownChangeColor">
                <div class="dropdown-trigger">
                    <button class="button" aria-haspopup="true" aria-controls="dropdown-menu">
                    <span>Warna</span>
                    <span class="icon is-small">
                        <i class="fa fa-angle-down" aria-hidden="true"></i>
                    </span>
                    </button>
                </div>
                <div class="dropdown-menu" id="dropdown-menu" role="menu">
                    <div class="dropdown-content">
                        <a @click="changeColor(cardId, 'red')" class="dropdown-item">
                            Merah
                        </a>
                        <a @click="changeColor(cardId, 'yellow')" class="dropdown-item">
                            Kuning
                        </a>
                        <a @click="changeColor(cardId, 'green')" class="dropdown-item">
                            Hijau
                        </a>
                        <a @click="changeColor(cardId, 'blue')" class="dropdown-item">
                            Biru
                        </a>
                    </div>
                </div>
            </div>
            <button class="button is-danger" @click="deleteTask(cardId)">Hapus</button>
        </div>
    </div>
</template>
<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  props: {
    cardId: String,
    title: String,
    description: String,
    color: String,
    status: String
  },
  data() {
      return {
          isActive: false,
          isActivatedChangeColor: false,
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
      }
  },
  methods: {
    toggleDropdown() {
        this.isActive = !this.isActive
    },
    toggleDropdownChangeColor () {
        this.isActivatedChangeColor = !this.isActivatedChangeColor 
    },
    async changeStatus (id, status) {
        try {
             axios({
                method: 'PUT',
                url: 'http://localhost:3000/tasks',
                headers: {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }, 
                data: {
                    task_id: id,
                    title: this.title,
                    description: this.description,
                    status: status,
                    color: this.color
                }
            })
            this.$parent.getTasks()
        } catch (error) {
            // eslint-disable-next-line
            console.log(error)   
        }
    },
    async changeColor (id, color) {
        try {
             axios({
                method: 'PUT',
                url: 'http://localhost:3000/tasks',
                headers: {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }, 
                data: {
                    task_id: id,
                    title: this.title,
                    description: this.description,
                    status: this.status,
                    color: color
                }
            })
            this.$parent.getTasks()
        } catch (error) {
            // eslint-disable-next-line
            console.log(error)   
        }
    },
    deleteTask (id) {
        axios({
            method: 'DELETE',
            url: 'http://localhost:3000/tasks',
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            data: {
                task_id: id
            }
        })
        .then(response => {
            // eslint-disable-next-line
            console.log(response)
            this.$parent.getTasks()
        })
        .catch(error => {
            // eslint-disable-next-line
            console.log(error)
        })
    }
  }
}
</script>
<style lang="scss" scoped>
.card {
  margin-bottom: 10px;
}
</style>