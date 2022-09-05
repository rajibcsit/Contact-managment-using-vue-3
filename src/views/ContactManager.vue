<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold"> Contact Manager
          <router-link to="/contacts/add" class="btn btn-success btn-sm"> <i class="fa fa-plus-circle"> </i> New </router-link>
        </p>

        <p class="fst-italic">Contact management is the process of recording contacts’ details and tracking their interactions with a business. Such systems have gradually evolved into an aspect of customer relationship management (CRM) systems, which allow businesses to improve sales and service levels leveraging a wider range of data.</p>
        
        <div class="row">
              <div class="col-md-6">
                <div class="row">
                  <div class="col">
                    <input v-model="query" 
                      @change="filteredList(query)"
                      type="text" 
                      class="form-control" 
                      placeholder="Type here to search"
                     /> 
                    
                  </div>
                    
                </div>
              </div>
            </div>
      </div>
    </div>
  <!------------- Loading --------->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <LoadingComponent />
        </div>
      </div>
    </div>

  </div>

<!------------- Error messages --------->
  <div v-if="!loading && errorMessage">
    <div class="container mt-3">
      <div class="row">
        <div class="col">
         <p class="h4 text-danger fw-bold "> {{errorMessage}} </p>
        </div>
      </div>
    </div>
  </div>


    <div v-if="!isFilter && contacts.length > 0" class="row" >
      <div class="col-md-6" v-for="contact of contacts" :key="contact">
        <div class="card my-2 list-group-item-success shadow-lg" >
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img :src="contact.photo" alt="" class="contact-image">
              </div>
              <div class="col-sm-7">
                
               <ul class="list-group">
                <li class="list-group-item">Name :<span class="fw-bold">{{contact.name}}</span></li>
              <li class="list-group-item">Email :<span class="fw-bold">{{contact.email}}</span></li>
                <li class="list-group-item">Mobile :<span class="fw-bold">{{contact.mobile}}</span></li>

               </ul>
              </div>
              <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-warning my-1"> 
                  <i class=" fa fa-eye" ></i>
                  
                </router-link>

                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1"> 
                  <i class=" fa fa-pen"></i>
                </router-link>

                <button class="btn btn-danger my-1" 
                  v-on:click="clickDeleteContact(contact.id)">
                  <i class=" fa fa-trash"></i>
                </button>

              </div>
            </div>
          </div>
        </div>

      </div>
    </div>

    <div v-if="isFilter && filterContacts.length > 0" class="row" >
      <div class="col-md-6" v-for="contact of filterContacts" :key="contact">
        <div class="card my-2 list-group-item-success shadow-lg" >
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img :src="contact.photo" alt="" class="contact-image">
              </div>
              <div class="col-sm-7">
                
               <ul class="list-group">
                <li class="list-group-item">Name :<span class="fw-bold">{{contact.name}}</span></li>
              <li class="list-group-item">Email :<span class="fw-bold">{{contact.email}}</span></li>
                <li class="list-group-item">Mobile :<span class="fw-bold">{{contact.mobile}}</span></li>

               </ul>
              </div>
              <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-warning my-1"> 
                  <i class=" fa fa-eye" ></i>
                  
                </router-link>

                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1"> 
                  <i class=" fa fa-pen"></i>
                </router-link>

                <button class="btn btn-danger my-1" 
                  v-on:click="clickDeleteContact(contact.id)">
                  <i class=" fa fa-trash"></i>
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
import { ContactService } from '@/services/ContactService';
import LoadingComponent from '@/components/LoadingComponent.vue';

  export default {
    name: 'ContactManager',
    components: {LoadingComponent},
    data : function () {
      return {
        loading : false,
        contacts : [],
        filterContacts : [],
        errorMessage: null,
        isFilter:false,
        

      }
    },
    created : async function ()  {

        try {
          this.loading = true;
          let response = await  ContactService.getAllContacts();
          this.contacts = response.data;
          this.loading = false;
        }
        catch (error){
          this.errorMessage = error;
          this.loading = false;
        }
    },
    methods : {
      clickDeleteContact: async function (contactId) {
        try{
           this.loading = true;
          let response = await ContactService.deleteContact(contactId);
            if(response){

              let response = await  ContactService.getAllContacts(); //getting fresh data
              this.contacts = response.data;
              this.loading = false;
            }
        }
        catch(error){
          this.errorMessage = error;
          this.loading = false;
          
        }
      },

    // Search Filtering contacts .........................//

      filteredList(searchValue) {
        this.filterContacts = this.contacts
        if(searchValue){
          this.isFilter = true
          let filterData =  this.filterContacts.filter(user => {
            return user.name.toLowerCase().includes(searchValue.toLowerCase())
          })
          return this.filterContacts = filterData
        } else this.isFilter = false
        return this.contacts;

      },

    },
    
  }
</script>

<style >

</style>