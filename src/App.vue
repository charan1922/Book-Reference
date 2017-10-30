  <template>
    <div id="app" class="container">
      <!-- built files will be auto injected -->
      <div class="page-header">
        <h1> Vue and Firebase Application</h1>
      </div>

        <div class="panel panel-default">

          <div class="panel-heading">
              <h3 >Add Book</h3>

          </div>

        <div class="panel-body">

          <form id="form" class="form-inline" v-on:submit.prevent="addBook" >


            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" class="form-control" id="bookTitle" v-model="newBook.title">
            </div>

            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" class="form-control" id="bookAuthor" v-model="newBook.author">
            </div>

            <div class="form-group">
              <label for="bookUrl">Url:</label>
              <input type="text" class="form-control" id="bookUrl" v-model="newBook.url">
            </div>
<input type="submit" class="btn btn-primary" value="Add Book">
          </form>

        </div>
  </div>


      <div class="panel panel-primary">

      <div class="panel-heading">
          <h3 >Books Lists</h3>
        </div>


   <div class="panel-body">
     <table class="table table-striped">
      <thead>
        <tr>
          <th>Title</th>
          <th>Author</th>
          <th>delete</th>
        </tr>
      </thead>
<tbody>
  <tr v-for="book in books">
    <td><a v-bind:href="book.url" target="_blank">{{book.title}}</a></td>
    <td>{{book.author}}</td>
    <td><span class="glyphicon glyphicon-trash" v-on:click="removeButton(book)"></span></td>
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
    apiKey: "AIzaSyCZ6TfNIMVKHDYuBxVwRd-tvg_IVY0KyHc",
    authDomain: "vuejs-firebase-be357.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-be357.firebaseio.com",
    projectId: "vuejs-firebase-be357",
    storageBucket: "vuejs-firebase-be357.appspot.com",
    messagingSenderId: "567116007265"
  };

  let app = Firebase.initializeApp(config);
  let db = app.database();

  let bookRef = db.ref("books");

export default {
  name: 'app',
  firebase: {
    books :bookRef
  },
data(){
  return {
    newBook:{
      title:'',
      author:'',
      url:''
    }
  }
},

methods:{
  addBook: function(){
    bookRef.push(this.newBook);
    toastr.success("Book Added");
    this.newBook.title='',
    this.newBook.author='',
    this.newBook.url=''
  },

  removeButton : function(book) {
    bookRef.child(book['.key']).remove();
    toastr.success("Book Removed");
  }
}

}
</script>

<style>
#app,th {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
