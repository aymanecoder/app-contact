<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 fw-bold text-success">
          Edit Contact
        </p>
        <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Odit asperiores accusantium,
          adipisci dignissimos provident explicabo nam officiis nobis dolorum ratione dolores repellendus atque ducimus
          sint at consectetur omnis perspiciatis ipsam!</p>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row">

      <div class="col-md-4">
        <form @submit.prevent="updateSubmit()">
          <div class="mb-2">
            <input  required v-model="contact.name" type="text" class="form-control" placeholder="Name">
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
            <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option value="">Select Group</option>
              <option :value="group.id" v-for="group in groups" :key="group.id">{{ group.name }}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="Submit" class="btn btn-success" value="Update">
          </div>
        </form>
      </div>
      <div class="col-md-4">
        <img
          :src="contact.photo"
          alt="" class="contact-img">
      </div>

    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService'
import '../router/index'
export default {
  name: "EditContact",
  data: function () {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {},
      errorMessage: null,
      groups: []

    }
  },
  created: async function () {
    try {
      this.loading = true;
      let response = await ContactService.getContact(this.contactId);
      let groupResponse = await ContactService.getAllGroups();
      this.contact = response.data;
      this.groups = groupResponse.data;
      this.loading = false;
    }
    catch (e) {
      this.errorMessage = e;
      this.loading = false;
    }
  },
  methods: {
   async updateSubmit(){
          try{
              let response = await ContactService.updateContact(this.contact, this.contactId);
                if(response){
                  return this.$router.push('/');
                }
                else{
                    return this.$router.push(`/contacts/edit/${this.contactId}`); 
                }
          }
          catch (error) {
              console.log(error);
          }
    }
  }

}
</script>

<style>
</style>