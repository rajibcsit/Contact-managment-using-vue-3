<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold"> Add Contact </p>
        <p class="fst-italic">Contact management is the process of recording contacts’ details and tracking their interactions with a business.</p>
      </div>
    </div>
  

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
            <input required v-model="contact.email" type="email" class="form-control" placeholder="Email">
          </div>
          <div class="mb-2">
            <input required v-model="contact.mobile" type="number" class="form-control" placeholder="Mobile">
          </div>
          <div class="mb-2">
            <input required v-model="contact.date_birth" type="text" class="form-control" placeholder="Date of birth">
          </div>
          <div class="mb-2">
            <input required v-model="contact.company" type="text" class="form-control" placeholder="Company">
          </div>
          <div class="mb-2">
            <input required v-model="contact.title" type="text" class="form-control" placeholder="Title">
          </div>
          <div class="mb-2">
            <select v-model="contact.groupId" class="form-control" v-if=" groups.length > 0 ">
              <option value=""> Select Group </option>
              <option :value="group.id" v-for="group of groups" :key="group.id" > {{group.name}}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" class="btn btn-success" value="Create">
          </div>
        </form>

      </div>
      <div class="col-md-4">
        <img :src="contact.photo" alt="" class="contact-image">
      </div>

    </div>

  </div>
  
</template>

<script>
import { ContactService } from '@/services/ContactService'

  export default {
    name: 'AddContact',
    data: function(){
      return{
        contact : {
          name: '',
          photo: '',
          email: '',
          mobile: '',
          date_birth: '',
          company: '',
          title: '',
          groupId: ''
        },
        groups : []
      }
    },
    created: async function(){
        try {
          let response = await ContactService.getAllGroups();
          this.groups = response.data;
        }
        catch(error){
          console.log('error');
        }
    },
    methods : {
      submitCreate : async function(){
        try{
          let response = await ContactService.createContact(this.contact);
          if(response){
            return this.$router.push('/');
          }
          else{
            return this.$router.push('/contacts/add');
          }
        }
        catch(error){
          console.log(error);
        }
      }
    }
  }
</script>

<style >

</style>