<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold mt-1">Add Contact</p>
        <p class="fst-italic">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Impedit dicta, molestiae tenetur eaque sed odit natus, nihil magni exercitationem necessitatibus perferendis earum fugiat quas illo, rerum qui iusto explicabo. Sequi?</p>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row"> 
      <div class="col-md-4">
        <form @submit.prevent="submitCreate()">
          <div class="mb-2">
            <input required type="text" class="form-control" placeholder="Name" v-model="contact.name">
          </div>
          <div class="mb-2">
            <input required type="text" class="form-control" placeholder="Photo Url" v-model="contact.photo">
          </div>

          <div class="mb-2">
            <input required type="email" class="form-control" placeholder="Email" v-model="contact.email">
          </div>
          <div class="mb-2">
            <input required type="number" class="form-control" placeholder="Mobile" v-model="contact.mobile">
          </div>
          <div class="mb-2">
            <input required type="text" class="form-control" placeholder="Company Name" v-model="contact.company">
          </div>
          <div class="mb-2">
            <input required type="text" class="form-control" placeholder="Title" v-model="contact.title">
          </div>
          <div class="mb-2">
            <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option value="">Select Group</option>
              <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
            </select>
          </div>

          
          <div class="mb-2">
            <input type="submit" class="btn btn-success" value="Create">
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
import { ContactService } from '@/services/ContactService';
export default {
  data() {
    return {
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
      groups: []
    }
  },
  created: async function () {
    try {
      let response = await ContactService.getAllGroups();
      this.groups = response.data
    }catch(error) {
      console.log(error)
    }
  },
  methods: {
    async submitCreate() {
      try {
        let response = await ContactService.createContact(this.contact);
        if(response) {
          return this.$router.push('/')
        }else {
          return this.$router.push('/contacts/add')
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