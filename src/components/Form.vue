<template>
    <div class="form-container">
      <button  class="add-book-btn"     @click="toggleForm" v-if="!showForm">Add Book</button>
      <form v-if="showForm" @submit.prevent="submitForm">
        <label for="bookId">Book ID:(Only Digits)</label>
        <input type="numeber" v-model="bookId" id="bookId"   pattern="[0-9]+"><br><br>
  
        <label for="bookTitle">Book Title:</label>
        <input type="text" v-model="bookTitle" id="bookTitle"  pattern="^[a-zA-Z\s]+$"><br><br>
  
        <label for="author">Author:</label>
        <input type="text" v-model="author" id="author"  pattern="^[a-zA-Z\s]+$"><br><br>
  
        <label for="language">Language:</label>
        <input type="text" v-model="language" id="language"  pattern="^[a-zA-Z\s]+$"><br><br>
  
        <!-- <label for="publishedDate">Published Date:</label>
        <input type="date" v-model="formData.publishedDate" id="publishedDate" required><br><br> -->
  
        <label for="publishedBy">Published By:</label>
        <input type="text" v-model="publishedBy" id="publishedBy"  pattern="^[a-zA-Z\s]+$"><br><br>
  
        <label for="genre">Genre:</label>
        <input type="text" v-model="genre" id="genre"  pattern="^[a-zA-Z\s]+$"><br><br>
  
        <button  v-if="formMode ==='add'"  type="submit">Submit</button>
        <button v-if="formMode === 'edit'" type="submit">Update</button>
        <button type="button" @click="toggleForm">Cancel</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props:{
      editedForm:{
        type: Object,
        default : null,
      },
    },
    data() {
      return {
        showForm: false,
       
          bookId: "",
          bookTitle: "",
          author: "",
          language: "",
          publishedBy: "",
          genre: "",
        formMode:"add",
      };
    },
    methods: {
      toggleForm() {
        this.showForm = !this.showForm;
      },
      submitForm() {
        const formData = {
          bookId: this.bookId,
          bookTitle: this.bookTitle,
          author: this.author,
          language: this.language,
          publishedBy: this.publishedBy,
          genre: this.genre,
        
        };

        if(this.formMode === 'add'){
          if (
          !formData.bookId.trim() ||
          !formData.bookTitle.trim() ||
          !formData.author.trim() ||
          !formData.language.trim() ||
          !formData.publishedBy.trim() ||
          !formData.genre.trim()
        )
        {window.alert("please Enter All data");
        return;
      }
        this.$emit('emit-form',formData);
        
        }else if( this.formMode === 'edit'){
          if(this.bookId !== this.editedForm.bookId){
            formData.bookId = this.bookId; 
          }
          if(this.bookTitle !== this.editedForm.bookTitle){
            formData.bookTitle = this.bookTitle;
          }
          if(this.author !== this.editedForm.author){
            formData.author = this.author;
          }
          if(this.language !== this.editedForm.language){
            formData.language = this.language;
          }
          if(this.publishedBy !== this.editedForm.publishedBy){
            formData.publishedBy = this.publishedBy;
          }
          if(this.genre !== this.editedForm.genre){
            formData.genre = this.genre;
          }
          this.$emit('update-form', formData);
        }
        this.bookId= '';
        this.bookTitle = '';
        this.author = '';
        this.language = '';
        this.publishedBy = '';
        this.genre = '';
        this.showForm = false;
        
      },
    },
    watch:{
      editedForm:{
        immediate: true,
          handler(value){
            if(value){
              this.formMode= 'edit';
              this.bookId=value.book_id;
              this.bookTitle = value.book_title;
              this.author = value.author;
              this.language= value.language;
              this.publishedBy= value.published_by;
              this.genre = value.genre;
              this.showForm = true;
            }
            else{
              this.formMode='add';
              this.bookId = ""; // resetForm()
              this.bookTitle="";
              this.author = "";
              this.language = "";
              this.publishedBy = "";
              this.genre="";
            }

          
        }
      }
    }
  };
  </script>


<style>
/* Styling for the form container */
.form-container {
  margin: 20px;
}

/* Styling for the 'Add Book' button */
.add-book-btn {
  background-color: #3498db;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.add-book-btn:hover {
  background-color: #2980b9;
}

/* Styling for the form */
.book-form {
  margin-top: 20px;
}

.book-form label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.book-form input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.book-form button {
  background-color: #4caf50;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  margin-right: 10px;
}

.book-form button:last-child {
  background-color: #e74c3c;
}

.book-form button:hover {
  opacity: 0.8;
}

</style>
  