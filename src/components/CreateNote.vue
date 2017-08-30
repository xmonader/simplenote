<template>
  <div class='create'>
    <div class="modal-mask" v-if="showModal" @close="showModal = false">
      <div class="modal-wrapper">
        <div class="modal-container">
  
          <div class="modal-header">
            <slot name="header">
              Add New Note 
            </slot>
          </div>
  
          <div class="modal-body">
            <slot name="body">
              <input id="wrong" v-model="title" placeholder="title">
              <input v-model="content" placeholder="content">
              <button @click="create">Create Note</button>
            </slot>
          </div>
  
          <div class="modal-footer">
            <slot name="footer">
              
              <button  @click="showModal = false">
                  Close Modal
                </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
    <div class='createNote' @click="showModal = true">
  
      <img src="http://www.startuppassion.eu/wp-content/uploads/2017/03/plus-sign.png" class="plusImage" alt=""><br>
      <button class='newNote' >NEW NOTE</button>
    </div>
  </div>
</template>

<script>
  import gql from 'graphql-tag'
  const createNote = gql `
    mutation createNote($title: String!, $content: String) {
      createNote(title:$title, content:$content) {
        id
        title
        content
      }
    }
  `
  export default {
    data: () => ({
      title: '',
      content: '',
      showModal: false,
    }),
  
    // Attribute
    methods: {
      create() {
        const title = this.title
        const content = this.content
  
        this.title = ''
        this.content = ''
  
        // Mutation
        this.$apollo.mutate({
          mutation: createNote,
          variables: {
            title,
            content,
          },
          updateQueries: {
            allNotes: (prev, {
              mutationResult
            }) => {
              return {
                // append at head of list because we sort the posts reverse chronological
                allNotes: [mutationResult.data.createNote, ...prev.allNotes],
              }
            },
          },
        }).then((data) => {
          // Result
          console.log(data);
          this.showModal=false;
        }).catch((error) => {
          // Error
          console.error(error)
        })
      },
    },
  }
</script>

<style>
  .createNote {
    /* Add shadows to create the "card" effect */
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    transition: 0.3s;
    width: 300px;
    height: 300px;
    margin-top: 35px;
    float: left;
  }
  

  .createNote:hover {
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  }
  
  .newPost {
    border: none;
    color: gray;
    background-color: white;
  }
  
  .plusImage {
    opacity: 0.4;
    margin-top: 25%;
    width: 25%;
    height: 25%;
  }
  
  .modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 400px;
  height: 330px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
.modal-container > * {
  padding: 10px;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 15%;
}
.modal-enter {
  opacity: 0;
}
.modal-footer{
  float: right;
}
.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>