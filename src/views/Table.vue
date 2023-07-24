<template>
    <div id="app">
      <div class="search-container">
      <form @submit.prevent="performSearch">
      <input type="text" v-model="searchQuery" placeholder="Search by BookTitile ....">
      <button type="submit" > Search</button><br><br>
    </form>
    </div>
    <div class="book-form-container">
      <BookForm  @emit-form="handleFormData"  
            :editedForm="editedData" 
            @update-form="updateTable"/>
            </div>
            <br><br>

      <table class="book-table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Book</th>
                <th>Title</th>
                <th>Author</th>
                <!-- <th>PublishedDate</th> -->
                <th>Language</th>
                <th>PublishedBy</th>
                <th>Genre</th>
                <th>Edit</th>
                <th>Delete</th>

          </tr>
        </thead>
        <tbody>
            <tr v-for="(arr, index) in emptyArray"   :key="index">
              
            <td>{{ arr.id }}</td>
            <td>{{ arr.book_id }}</td>
            <td>{{ arr.book_title }}</td>
            <td>{{ arr.author}}</td>
            <td>{{ arr.language}}</td>
            <td>{{ arr.published_by }}</td>
            <td>{{ arr.genre }}</td>
            <td>
                <button @click="editForm(arr)">Edit</button>
            </td>
            <td>
                <button @click="deleteData(arr.id)">Delete</button>
            </td>


            </tr> 
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import BookForm from '../components/Form.vue';
  import axios from 'axios';
  export default {
    components: {
      BookForm,
    },
    data(){
        return{
            emptyArray:[],
            editedData:null,
            searchQuery:"",
        }
    },
    mounted(){
        this.getDetails();
    },
    methods:{
        handleFormData(value){
          console.log(value.bookId);
         
           axios
           .post(`http://127.0.0.1:3333/insert`,value)
           .then( (response) =>{
            console.log(response);
            this.emptyArray.push(response.data);
           })
          
           .catch( (error) =>{
            console.error("Error While Pushing or inserting Data");     
              });
          
           
        },
        getDetails(){
            axios
            .get(`http://127.0.0.1:3333/getting`)
            .then( (response) =>{
                this.emptyArray = response.data;
                console.log(this.emptyArray);
             })
             .catch( (error) =>{
                console.error( "Error while getting data ",error);
             });

        },
        deleteData(id){
            if (window.confirm("Are you sure you want to delete this data?")) {
        axios
          .delete(`http://127.0.0.1:3333/del/${id}`)
          .then((response) => {
            console.log(response.data);
            this.getDetails();
          })
          .catch((error) => {
            console.error("Error while deleting", error);
          });
      } else {
        console.log("Deletion canceled");
      } 
        },
        editForm(arr) {
  console.log("this is in editing mode ");
  this.formMode = 'edit'; // Corrected the property name (formMode instead of formData)
 this.editedData= {...arr};
},
        updateTable(value) {
  const id = this.editedData.id;

 
if(window.confirm("Are you sure you want to update this data?")){
  axios
    .put(`http://127.0.0.1:3333/upd/${id}`, value)
    .then((response) => {
      const updatedDataIndex = this.emptyArray.findIndex((item) => item.id === id);
      if (updatedDataIndex !== -1) {
        this.emptyArray[updatedDataIndex] = response.data;
      } else {
        console.error("Updated data not found in list ");
      }
    })
    .catch((error) => {
      console.error("Error while Updating  ", error);
    });

  this.editedData = null;
}else{
console.log("update canceled");
}
    },
performSearch(){
            console.log("Performing search");
      axios
        .get(`http://127.0.0.1:3333/search?searchQuery=${this.searchQuery}`)
        .then((response) => {
            console.log(response.data);
          this.emptyArray = response.data;
        })
        .catch((error) => {
          console.error("Error while searching", error);
        });




    }
        
    
   },
   
  };
  </script>


<style>
body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 0;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

/* Styles for the search form */
.search-container {
  text-align: center;
  margin-bottom: 20px;
}

.search-container form {
  display: inline-block;
}

.search-container input[type="text"] {
  padding: 8px;
  width: 200px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-container button {
  background-color: #3498db;
  color: #ffffff;
  border: none;
  padding: 8px 16px;
  margin-left: 10px;
  border-radius: 4px;
  cursor: pointer;
}

.search-container button:hover {
  background-color: #2980b9;
}

/* Styles for the book form */
.book-form-container {
  margin-bottom: 20px;
}

/* Styles for the book table */
.book-table {
  width: 100%;
  border-collapse: collapse;
}

.book-table th,
.book-table td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #cbc5c5;
}

.book-table th {
  background-color: #0a0303;
}

.book-table button {
  background-color: #4acf50;
  color: #070606;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.book-table button:last-child {
  background-color: #226e65;
}

.book-table button:hover {
  opacity: 0.8;
}

</style>
  