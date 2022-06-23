<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold mt-1">View Contact</p>
        <p class="fst-italic">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Impedit dicta, molestiae tenetur eaque sed odit natus, nihil magni exercitationem necessitatibus perferendis earum fugiat quas illo, rerum qui iusto explicabo. Sequi?</p>
      </div>
    </div>
  </div>

  <!-- sipnner -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <SpinnerLoader />
        </div>
      </div>
    </div>
  </div>

  <!-- Error message -->
  <div v-if="!loading && errorMessage">
  <div class="container mt-3">
      <div class="row">
        <div class="col">
          <p class="h4 text-danger fw-bold">{{errorMessage}}</p>
        </div>
      </div>
    </div>
  </div>

  <div class="container" v-if="!loading && isDone">
    <div class="row align-items-center">
      <div class="col-md-4">
        <img :src="contact.photo" alt="" class="contact-img-big">
      </div>
      <div class="col-md-6">
        <ul class="list-group">
                  <li class="list-group-item"> Name : <span class="fw-bold">{{contact.name}}</span> </li>
                  <li class="list-group-item"> Email : <span class="fw-bold">{{contact.email}}</span> </li>
                  <li class="list-group-item"> Mobile : <span class="fw-bold">{{contact.mobile}}</span> </li>
                  <li class="list-group-item"> Company : <span class="fw-bold">{{contact.company}}</span> </li>
                  <li class="list-group-item"> Title : <span class="fw-bold">{{contact.title}}</span> </li>
                  <li class="list-group-item"> Group : <span class="fw-bold">{{group.name}}</span> </li>
                </ul>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col">
        <router-link to="/" class="btn btn-success"> <i class="fa fa-arrow-alt-circle-left"></i> Back</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService'
import SpinnerLoader from '@/components/SpinnerLoader.vue';

export default {
  components: { SpinnerLoader },
    data() {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            errorMessage: null,
            group: {}
        };
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContacts(this.contactId);
            let groupResponse = await ContactService.getGroups(response.data);
            this.contact = response.data;
            this.group = groupResponse.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;
        }
    },
    methods: {
      isDone: function () {
        return Object.keys(this.contact).lenght > 0 && Object.keys(this.group).lenght > 0;
      }
    }
}
</script>

<style>

</style>