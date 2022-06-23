<template>
<div class="container mt-3">
  <div class="row">
    <div class="col">
      <p class="h3 fw-bold text-success">
          Add Contact
      </p>
      <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Earum, vitae laudantium? Fugiat quibusdam recusandae laborum aliquid ex officiis asperiores, a magni, quos ducimus deserunt, animi ipsum vel qui incidunt velit.</p>
    </div>
  </div>
</div>
<div class="container mt-3">
  <div class="row">
  
      <div class="col-md-4">
        <form @submit.prevent="submitCreate()">
            <div class="mb-2">
              <input required v-model="contact.name" type="text" class="form-control" placeholder="Name">
            </div>
              <div class="mb-2">
              <input required v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL">
            </div>
              <div class="mb-2">
              <input required v-model="contact.email" type="text" class="form-control" placeholder="Email">
            </div>
              <div class="mb-2">
              <input required v-model="contact.mobile" type="number" class="form-control" placeholder="Mobile">
            </div>
             <div class="mb-2">
              <input required v-model="contact.company" type="text" class="form-control" placeholder="Company">
            </div>
             <div class="mb-2">
              <input required v-model="contact.title" type="text" class="form-control" placeholder="Title">
            </div>
             <div class="mb-2">
              <select required v-model="contact.groupId" class="form-control" v-if="groups.length>0">
                <option value="">Select Group</option>
                <option :value="group.id" v-for="group in groups" :key="group.id">{{group.name}}</option>
              </select>
            </div>
             <div class="mb-2">
              <input type="Submit" class="btn btn-success" value="Create">
            </div>
        </form>
      </div>
        <div class="col-md-4">
          <img  :src="contact.photo" alt="" class="contact-img">
        </div>
    
  </div>
 
</div>
</template>

<script>
import { ContactService } from '@/services/ContactService'
import  '../router/index';
export default {
    name: "AddContact",
    data : function () {
     return{
      contact : {
          name : '',
          photo : '',
          email : '',
          mobile : '',
          company : '',
          title : '',
          groupId : '',




      },
      groups :[]
      }
     
    },
    methods:{
        async submitCreate(){
          try{
              let response = await ContactService.createContact(this.contact);
              if(response){
                return this.$router.push('/');
              }
              else{
                return this.$router.push('/contacts/add');  
              }
          }
          catch(e){
            console.log(e);
          }
        }
     },
    created :async function () {
        try{
          let response = await ContactService.getAllGroups();
          this.groups = response.data;
        }
        catch(e) {
          console.log(e);
        }
    }
}
</script>

<style scoped>

</style>