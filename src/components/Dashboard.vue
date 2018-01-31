<template>
  <div id="dashboard">
    <h3>All Tasks</h3>
    <ul>

      <li v-for="task in tasks" v-bind:key="task.id" class="collection-item">
        <form action="#">
          <input type="checkbox" class="filled-in" v-bind:id="task.id" v-model="task.completed" />
          <label v-bind:for="task.id">{{ task.description }}</label>
        </form>
      </li>
    </ul>


    <h4>Incomplete Tasks</h4>
    <ul>
      <li v-for="task in incompleteTasks" v-text="task.description"></li>
    </ul>

    <h4>Complete Tasks</h4>
    <ul>
      <li v-bind:class="{ complete: task.completed }" v-for="task in completeTasks" v-text="task.description"></li>
    </ul>

  <div class="fixed-action-btn">
    <router-link to="/new" class="btn-floating btn-large orange">
      <i class="fa fa-plus"></i>
    </router-link>
  </div>

  </div>
</template>


<script>
import db from './firebaseInit'
export default {
  name: 'dashboard',
  data () {
      return {
        tasks: []
      }
    },
  created () {
      db.collection('tasks').orderBy('description').get().then(querySnapshot => {
        querySnapshot.forEach(doc => {
          // console.log(doc.data())
          const data = {
            'id': doc.id,
            'description': doc.data().description,
            'completed': doc.data().completed
          }
          this.tasks.push(data)
        })
      })
  },

  methods: {

  },

  computed: {
    incompleteTasks() {
      return this.tasks.filter(task => ! task.completed);
    },
    completeTasks() {
      return this.tasks.filter(task => task.completed);
    }
  }
}
</script>
