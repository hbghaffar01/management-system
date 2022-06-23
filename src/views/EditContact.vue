<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold mt-1">Edit Contact</p>
        <p class="fst-italic">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Impedit dicta, molestiae tenetur eaque sed odit natus, nihil magni exercitationem necessitatibus perferendis earum fugiat quas illo, rerum qui iusto explicabo. Sequi?</p>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row"> 
      <div class="col-md-4">
        <form @submit.prevent="updateSubmit()">
          <div class="mb-2">
            <input required v-model="contact.name" type="text" class="form-control" placeholder="Name">
          </div>
          <div class="mb-2">
            <input required v-model="contact.photo" type="text" class="form-control" placeholder="Photo Url">
          </div>
          <div class="mb-2">
            <input required v-model="contact.email" type="email" class="form-control" placeholder="Email">
          </div>
          <div class="mb-2">
            <input required v-model="contact.mobile" type="number" class="form-control" placeholder="Mobile">
          </div>
          <div class="mb-2">
            <input required v-model="contact.company" type="text" class="form-control" placeholder="Company Name">
          </div>
          <div class="mb-2">
            <input required v-model="contact.title" type="text" class="form-control" placeholder="Title">
          </div>
          <div class="mb-2">
            <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" class="btn btn-success" value="Update">
          </div>
          
        </form>
      </div>
      <div class="col-md-4">
        <img :src="contact.photo" alt="" class="contact-img">
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService'

export default {
  data() {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {
        name : '',
        photo: '',
        email: '',
        mobile: '',
        company: '',
        url: '',
        title: '',
        groupId: '',
      },
      errorMessage: null,
      groups: []
    }
  },
  created: async function() {
    try {
      this.loading = true;
      let response = await ContactService.getContacts(this.contactId);
      let groupResponse = await ContactService.getAllGroups();
      this.contact = response.data;
      this.groups = groupResponse.data;
      this.loading = false;
    } catch(error){
      this.errorMessage = error;
      this.loading = false; 
    }
  },
  methods: {
    async updateSubmit() {
      try {
        let response = await ContactService.updateContact(this.contact, this.contactId);
        if(response) {
          return this.$router.push('/')
        }else {
          return this.$router.push(`/contacts/edit/${contact.id}`)
        }
      }catch(error) {
        console.log(error)
      }
    }
  }
}
</script>

<style>

</style>