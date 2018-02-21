<template>
  <div id="app">
    <h1>Vue and FireBase</h1>

    <div class="card">
      <div class="card-header">
        <h3>Add a Link</h3>
      </div>

      <div class="card-body">
        <form v-on:submit.prevent="addLink" class="form-inline">
          <div class="form-group">
            <label for="">Title</label>
            <input type="text" class="form-control" name="" value="" placeholder="Title" v-model="newLink.title">
          </div>
          <div class="form-group">
            <label for="">Author</label>
            <input type="text" class="form-control" name="" value="" placeholder="Author" v-model="newLink.author">
          </div>
          <div class="form-group">
            <label for="">url</label>
            <input type="text" class="form-control" name="" value="" placeholder="Url" v-model="newLink.url">
          </div>

          <input type="submit" name="button" value="Add a Link"class="btn bnt-success">
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
            <tr v-for="link of links">
              <td><a v-bind:href="link.url" target="_blank">{{link.title}}</a></td>
              <td>{{link.author}}</td>
              <td>
                <button type="button" name="button" class="btn btn-danger" v-on:click="deleteLink(link)"><i class="fa fa-trash-o"></i> </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import FireBase from 'firebase';
import toastr from 'toastr';

let config = {
  apiKey: "AIzaSyD2V68eWdz14f6ew0eLijztjFvxr_UR9w0",
  authDomain: "vue-firebase-duck.firebaseapp.com",
  databaseURL: "https://vue-firebase-duck.firebaseio.com",
  projectId: "vue-firebase-duck",
  storageBucket: "vue-firebase-duck.appspot.com",
  messagingSenderId: "111858542278"
};

let app = FireBase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
  name: 'App',
  firebase: {
    links: linksRef
  },
  data() {
    return {
      newLink: { title: '', author: '', url: '' }
    }
  },
  methods: {
    addLink() {
      linksRef.push(this.newLink);
      this.newLink = {};
    },
    deleteLink(link) {
      linksRef.child(link['.key']).remove();
      // Display an info toast with no title
      toastr.info('Are you the 6 fingered man?')
      toastr.success('Link Removed');
      // Display a warning toast, with no title
      toastr.warning('My name is Inigo Montoya. You killed my father, prepare to die!')

      // Display a success toast, with a title
      toastr.success('Have fun storming the castle!', 'Miracle Max Says')

      // Display an error toast, with a title
      toastr.error('I do not think that word means what you think it means.', 'Inconceivable!')

      // Immediately remove current toasts without using animation
      //toastr.remove()

      // Remove current toasts using animation
      //toastr.clear()

      // Override global options
      toastr.success('We do have the Kapua suite available.', 'Turtle Bay Resort', {timeOut: 5000})
    }
  }
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
