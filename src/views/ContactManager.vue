<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h2 text-success fw-bold">
          Contact Manager
          <router-link to="/contacts/add" class="btn btn-success btn-sm"><i class="fa fa-plus-circle"></i> New
          </router-link>
        </p>
        <p class="fst-italic">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Veniam
          recusandae facere rerum perspiciatis est odit ab iure eius officia,
          veritatis voluptatum nemo vitae, commodi architecto laudantium! Totam
          sunt dicta sequi.
        </p>
        <form>
          <div class="row">
            <div class="col-md-6">
              <div class="row">
                <div class="col">
                  <input type="text" v-model="search" class="form-control" placeholder="Search Name" />
                </div>
                <div class="col">
                  <input type="submit" class="btn btn-outline-dark" value="Search" />
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
  <!-- Spinner -->
  <div class="container" v-if="loading">
    <div class="row">
      <div class="col">
        <Spinner />
      </div>
    </div>

  </div>

  <!-- ErrorMessage -->
  <div v-if="!loading && errorMessage">
    <div class="container mt-3">
      <div class="row">
        <div class="col">
          <p class="h3 fw-bold text-danger">{{ errorMessage }}</p>
        </div>
      </div>
    </div>

  </div>


  <div class="container mt-3 " v-if="contacts.length > 0">
    <div class="row">
      <div class="col-md-6" v-for="contact in contacts" :key="contact">
        <div class="card my-2 list-group-item-success shadow-lg">
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img :src="contact.photo" alt="" class="contact-img">
              </div>
              <div class="col-sm-7">
                <ul class="list-group">
                  <li class="list-group-item">Name : <span class="fw-bold">{{ contact.name }}</span></li>
                  <li class="list-group-item">Mobile : <span class="fw-bold">{{ contact.mobile }}</span></li>
                  <li class="list-group-item">Email : <span class="fw-bold">{{ contact.email }}</span></li>

                </ul>
              </div>
              <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-warning my-1"><i class="fa fa-eye"></i>
                </router-link>
                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1"><i class="fa fa-pen"></i>
                </router-link>
                <button class="btn btn-danger my-1" @click="Delete(contact.id)">
                  <i class="fa fa-trash"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService'
import Spinner from '../components/Spinner';
import '../router/index';
 

export default {
  name: "ContactManager",
  components: { Spinner },

  data: function () {
    return {
      loading: false,
      contacts: [],
      errorMessage: null,
      search: ''
    }
  },
  created: async function () {
    try {
      this.loading = true;
      let response = await ContactService.getAllContacts();
      this.contacts = response.data;
      this.loading = false;
    }
    catch (error) {
      this.errorMessage = error;
      this.loading = false;
    }

  },
  methods: {
    async Delete(contactId) {
      try {
        this.loading= true;
        let response = await ContactService.DeleteContact(contactId);
        if (response) {
          let response = await ContactService.getAllContacts();
          this.contacts = response.data;
          this.loading = false;
        }
      }
      catch (error) {
        this.errorMessage = error;
        this.loading = false;
      }
    },
   

 
  },
 

  


}

</script>

<style>
</style>
