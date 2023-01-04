<template>
  
    <div class="css1">
        <div class="mt-3">
     
    
    
    
      
        <table class="table table-striped table-bordered table-sm">
          <thead class="thead-light">
        <th>Student Name</th>
        <th>Student Age</th>
          </thead>
        <tr v-for="item in submittedNames" :key="item.name">
          <td>
            <span>{{item.name}} </span>
          </td>
          <td>
            <span>{{item.age}} </span>
          </td>
          <button v-on:click.prevent="removeTask(index)" >Remove</button>
        </tr>
      </table>
      </div>
      <b-button v-b-modal.modal-add>Add/Remove Student</b-button>
      <b-button v-b-modal.modal-update>Update</b-button>
      
      
      <b-modal id="modal-update" hide-footer ref="modal" title="Update Student Name">
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Name"
            label-for="name-input"
            invalid-feedback="Name is required"
            :state="nameState"
          >
            <b-form-input
              id="name-input"
              v-model="name"
              :state="nameState"
              required
            ></b-form-input>
            
          </b-form-group>
          <b-form-group
            label="New Name"
            label-for="new-name-input"
            invalid-feedback="Name is required"
            :state="nameState"
          >
            <b-form-input
              id="new-name-input"
              v-model="newname"
              :state="nameState"
              required
            ></b-form-input>
            
          </b-form-group>
        </form>
        <b-button class="mt-2"  pill variant="success"  block @click="handleupdate">Update</b-button>

      </b-modal>
      <b-modal
        id="modal-add"
        hide-footer
        ref="modal"
        title="Add/Remove Student"
        @show="resetModal"
        @hidden="resetModal"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Name"
            label-for="name-input"
            invalid-feedback="Name is required"
            :state="nameState"
          >
            <b-form-input
              id="name-input"
              v-model="item.name"
              :state="nameState"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            label="Age"
            label-for="age-input"
            invalid-feedback="Age is required"
            :state="nameState"
          >
          <b-form-input
              id="age-input"
              v-model="item.age"
              type="number"
              min="1"
              :state="nameState"
              
            ></b-form-input>
          </b-form-group>


          
        </form>
        <b-button class="mt-2"  pill variant="success"  block @click="handleOk">Add</b-button>
        <b-button class="mt-2" pill variant="danger" block @click="handleremove">Remove</b-button>
      </b-modal>
    </div>
  </template>
  
  <script>
    export default {
      mounted(){
        this.fetchStudents();
      },
        
      data() {
        return {
          
          newname: '',
          nameState: null,
          
          submittedNames: [],
          item:{name:"", age:""},
          
        }
        
      },
      methods: {

        async fetchStudents(){
          const students = await this.$axios.$get('http://localhost:3001/getStudents')
          this.studentList = studentList;
        },


        checkFormValidity() {
          const valid = this.$refs.form.checkValidity()
          this.nameState = valid
          return valid
        },
        removeTask(item) {
            //this.$delete(this.tasks, index);
            this.submittedNames.splice(item, 1);
        },
        resetModal() {
          this.item.name = ''
          this.item.age = ''
          this.nameState = null
        },
        handleOk(bvModalEvent) {
          // Prevent modal from closing
          bvModalEvent.preventDefault()
          // Trigger submit handler
          this.handleSubmit()
        },
        handleremove(bvModalEvent) {
          // Prevent modal from closing
          bvModalEvent.preventDefault()
          // Trigger submit handler
          if (!this.checkFormValidity()) {
            return
          }
          this.submittedNames.splice(this.submittedNames.indexOf(this.item.name), 1)
          this.$nextTick(() => {
            this.$bvModal.hide('modal-add')
          })
        },
        handleupdate(bvModalEvent) {
          // Prevent modal from closing
          bvModalEvent.preventDefault()
          // Trigger submit handler
          if (!this.checkFormValidity()) {
            return
          }
          this.submittedNames.splice(this.submittedNames.indexOf(this.name), 1, this.newname)
          this.$nextTick(() => {
            this.$bvModal.hide('modal-update')
          })
         
        },

        
        handleSubmit() {
          // Exit when the form isn't valid
        
          // Push the name to submitted names
          this.submittedNames.push({...this.item})
          // Hide the modal manually
          this.item = [];
          console.log(this.submittedNames.indexOf(this.item.name))
          
          this.$nextTick(() => {
            this.$bvModal.hide('modal-add')
          })
        }
      }
    }
  </script>
 
 
 
 
 
 <style>
.css1{
    margin: 50px;
}    

</style>