<template>
  <div id="app" class="container">
    <h1>Vue and Firebase ♫</h1>
    <div class="card mt-5">
      <div class="card-header">
        <h3>Add Link</h3>
      </div>
      <div class="card-body ">
        <form v-on:submit.prevent = "addlink()">  
          <div class="form-group">
            <label for="title">Title</label>
            <input class="form-control"
              v-model="newLink.title"
              type="text" 
              placeholder="Ingrese el titulo del Link">
          </div>
          <div class="form-group">
            <label for="author">Author</label>
            <input class="form-control"
              v-model="newLink.author"
              type="text" 
              placeholder="Ingrese el Nombre del Autor">
          </div>
          <div class="form-group">
            <label for="url">URL:</label>
            <input class="form-control"
              v-model="newLink.url"
              type="text" 
              placeholder="Ingrese su URL">
          </div>
          <button type="submit" class="btn btn-primary">Guardar</button>
        </form>
      </div>
    </div>

    <hr>

    <div class="card">

      <div class="card-header">
        <h3 class="card-title">Link List</h3>
      </div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="link in links" :key="link._id">
              <td>
                <a v-bind:href="link.url">{{link.title}}</a>
              </td>
              <td>{{link.author}}</td>
              <td>
                <button v-on:click="deleteLink(link)" 
                  class="btn btn-danger">
                  <i class="fa fa-trash-o"></i>
                </button>
                </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

import Firebase,{ functions } from 'firebase';

var config = {
    apiKey: "AIzaSyBQoQSm9DHcXxly6zQqAGoU3U4ISGn_sCs",
    authDomain: "vuefire-f35a1.firebaseapp.com",
    databaseURL: "https://vuefire-f35a1.firebaseio.com",
    projectId: "vuefire-f35a1",
    storageBucket: "",
    messagingSenderId: "110658761482"
  };

var app = Firebase.initializeApp(config);
var db = app.database();
var linksRef = db.ref('links');


export default {
  name: 'App',
  data() {
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      } 
    }
  },
  firebase:{
    links: linksRef
  },
  methods: {
    addlink () {
      linksRef.push(this.newLink);
      this.newLink.title='';
      this.newLink.author='';
      this.newLink.url='';
    },
    deleteLink: function (link) {
      linksRef.child(link['.key']).remove();
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
