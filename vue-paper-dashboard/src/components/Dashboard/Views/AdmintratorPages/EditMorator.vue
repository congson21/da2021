<template>
  <div class="card">
    <div class="card-header">
      <h4 class="title"><span v-if="!view">Edit</span>
         Moderator Profile</h4>
    </div>
    <div class="card-body">
      <form>
        <!-- <div class="row">
          <div class="col-md-12">
           <img src="/static/img/mike.jpg" alt="W3Schools.com" width="100" height="132">
          </div>
        </div> -->
        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="ID"
              :disabled="true"
              placeholder="id"
              v-model="userID"
            >
            </fg-input>
          </div>
          
        </div>

        <div class="row">
          <div class="col-md-6">
            <fg-input
            :disabled="view"
              type=""
              label="Moderator's name"
              placeholder="name"
              v-model="user.full_name"
            >
            </fg-input>
          </div>
          <div class="col-md-6">
            <fg-input
            :disabled="view"
              type="text"
              label="Email"
              placeholder="email"
              v-model="user.email"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-6">
            <fg-input
            :disabled="view"
              type="text"
              label="DOB"
              placeholder="dob"
              v-model="user.dob"
            >
            </fg-input>
          </div>
          <div class="col-md-6">
            <fg-input
            :disabled="view"
              type="text"
              label="Phone number"
              placeholder="phone number"
              v-model="user.phone_number"
            >
            </fg-input>
          </div>
        </div>
       
        <div class="text-center">
          <p-button v-on:click="EnableEdit" v-if="view" type="info">Edit Profile</p-button>
          <button
          v-if="!view"
            type="submit"
            class="btn btn-info btn-fill btn-wd"
            @click.prevent="updateProfile"
          >
            Update Profile
          </button>
         
        </div>
        <div class="clearfix"></div>
      </form>
    </div>
  </div>
</template>
<script>
import { GET_MODERATOR } from "../../../../../src/constants/api.js";
import axios from "axios";
export default {
  data() {
    return {
      user: {
        dob: null,
        email: null,
        full_name: null,
        phone_number: null,
      },
      userID: this.$route.params.user_id,
      view : true,
    };
  },
  methods: {
    updateProfile() {
      
    },
    EnableEdit(){
      this.view = false
    },
    
    async getDetail() {
      console.log("vào getdetail")
      try {
        const response = await axios.get(GET_MODERATOR, {
          params: { user_id: this.userID },
        });
        const detail = response.data.data;
        console.log("detail" , detail)
        (this.user.dob = detail.dob),
          (this.user.email = detail.email),
          (this.user.full_name = detail.full_name),
          (this.user.phone_number = detail.phone_number),
          console.log("this ", this.user);
      } catch (error) {
        console.log(error);
        // await this.$notify({
        //   type: "danger",
        //   message: "Server is not responsed",
        // });
        // return;
      }
    },

  },
  created() {
    console.log("thissss", this.userID);
    this.getDetail();
  },
};
</script>
<style>
</style>
