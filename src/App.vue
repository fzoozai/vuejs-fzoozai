<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase <small>Got shit done finally! fzoozaitech.io</small></h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Wer wird Fußballer des Jahres?</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Spieler:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Position:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Player">
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Spieler Liste</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
          <tr>
            <th>Spieler</th>
            <th>Position</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="book in books">
            <td><a v-bind:href="book.url">{{book.title}}</a></td>
            <td>{{book.author}}</td>
            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>


<script>
  import Hello from './components/Hello'
  import Firebase from 'firebase'
  import toastr from 'toastr'

  let config = {
    apiKey: "AIzaSyACRk573MpeAy7YUvloBAWWeQw9bjD74rQ",
    authDomain: "vuejs-firebase-fz.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-fz.firebaseio.com",
    storageBucket: "vuejs-firebase-fz.appspot.com",
    messagingSenderId: "213841374257"
  };

  let app = Firebase.initializeApp(config);
  let db = app.database();
  let booksRef = db.ref('books');

  export default {
    name: 'app',
    firebase: {
      books: booksRef
    },

    data () {
      return {
          newBook: {
          title: '',
          author: '',
          url: 'http://'
        }
      }
    },

    methods: {
      addBook: function () {
        booksRef.push(this.newBook);
        this.newBook.title = '';
        this.newBook.author = '';
        this.newBook.url = 'http://';
      },
      removeBook: function (book) {
        booksRef.child(book['.key']).remove()
        toastr.success('Book removed successfully')
      }
    },

    components: {
      Hello
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
