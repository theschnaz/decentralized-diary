<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <div class="col-md-8 col-md-offset-2">
          <h1 class="page-header">Write a note to future you
            <small><span class="sign-out">(<a href="#" @click.prevent="signOut">Sign Out</a>)</span></small>
          </h1>
          <h2 class="user-info">
            <small class="pull-right">
            {{ user.username ? user.username : user.identityAddress }}
            </small>

          </h2>
          Dear Diary,<br /><br />
          <form @submit.prevent="addTodo" :disabled="! todo">
            <div class="input-group">
              <textarea style="height:250px;" v-model="todo" class="form-control" placeholder="Dear Diary," autofocus></textarea>
              <span class="input-group-btn">
                <button class="btn btn-default" type="submit" :disabled="! todo">Add</button>
              </span>
            </div>
          </form>

          <ul class="list-group">
            <li v-for="todo in todos"
              class="list-group-item"
              :class="{completed: todo.completed}"
              :key="todo.id">
              <label>
                <a @click.prevent="todos.splice(todos.indexOf(todo), 1)"
                class="delete pull-right"
                href="#">X</a>
                Deary Diary,<br /><br />
                {{ todo.text }}
              </label>
            </li>
          </ul>

        </div>
      </div>
      <center>
        <img src="https://78.media.tumblr.com/775de8eb8487af171fa0cf9ad1620f23/tumblr_o03a0qc0Ne1v05zj7o1_500.gif" style="width:200px;" />
      </center>
    </div>
    <small class="creds">
      Created by <a href="http://twitter.com/theschnaz" target="_blank">Gregory Schnese</a> Source code on <a href="https://github.com/blockstack/blockstack-todos" target="_blank">Github</a>
    </small>
  </div>
</template>

<script>
var STORAGE_FILE = 'todos.json'

export default {
  name: 'dashboard',
  props: ['user'],
  data () {
    return {
      blockstack: window.blockstack,
      todos: [],
      todo: '',
      uidCount: 0
    }
  },
  watch: {
    todos: {
      handler: function (todos) {
        const blockstack = this.blockstack
        const encrypt = true
        return blockstack.putFile(STORAGE_FILE, JSON.stringify(todos), encrypt)
      },
      deep: true
    }
  },
  mounted () {
    this.fetchData()
  },
  methods: {
    addTodo () {
      if (!this.todo.trim()) {
        return
      }
      this.todos.unshift({
        id: this.uidCount++,
        text: this.todo.trim(),
        completed: false
      })
      this.todo = ''
    },

    fetchData () {
      const blockstack = this.blockstack
      const decrypt = true
      blockstack.getFile(STORAGE_FILE, decrypt)
      .then((todosText) => {
        var todos = JSON.parse(todosText || '[]')
        todos.forEach(function (todo, index) {
          todo.id = index
        })
        this.uidCount = todos.length
        this.todos = todos
      })
    },

    signOut () {
      this.blockstack.signUserOut(window.location.href)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

input::placeholder {
  color: grey;
}

label {
  margin-bottom: 0;
  // width: 100%;
  cursor: pointer;
  input[type="checkbox"] {
    margin-right: 5px;
  }
}
.list-group-item {
  &.completed label {
    text-decoration: line-through;
  }

  .delete {
    display: none;
  }

  &:hover .delete {
    display: inline;
    color: grey;
    &:hover {
      text-decoration: none;
      color: red;
    }
  }
}
</style>
