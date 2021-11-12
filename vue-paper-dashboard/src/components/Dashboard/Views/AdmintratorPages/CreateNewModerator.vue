<template>
  <div class="card">
    <div class="card-header">
      <h4 class="title">Create New Moderator Account</h4>
    </div>
    <div class="card-body">
      <form  @submit.prevent="createNewModerator">
        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Username"
              placeholder="Enter username"
              v-model="username"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Full name"
              placeholder="Enter name"
              v-model="fullname"
            >
            </fg-input>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Email"
              placeholder="Enter email"
              v-model="email"
            >
            </fg-input>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Phone"
              placeholder="Enter phone"
              v-model="phonenumber"
            >
            </fg-input>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <datepicker
              lable="DOB"
              :format="format"
              v-model="selectedDOB"
              class="mb-2"
              placeholder="DOB"
            ></datepicker>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Password"
              placeholder="Enter password"
              v-model="password"
            >
            </fg-input>
          </div>
        </div>
        <!-- <div class="row">
          <div class="col-md-6">
            <p class="mb-2 mt-1">Store's logo</p>
                    <input type="file" @change="onFileSelected" />
                    <div id="preview">
                      <img v-if="url" :src="url" />
                    </div>
          </div>
          
        </div> -->

        <div class="text-center">
          <button
            type="submit"
            class="btn btn-info btn-fill btn-wd"
          >
            Save
          </button>
        </div>
        <div class="clearfix"></div>
      </form>
    </div>
  </div>
</template>
<script>
import { CREATE_EMPLOYEE } from "../../../../../src/constants/api.js";
import Datepicker from "vuejs-datepicker/dist/vuejs-datepicker.esm.js";
import axios from "axios";
export default {
  components: {
    Datepicker,
  },
  data() {
    return {
     
      username: null,
      fullname: null,
      email: null,
      phonenumber: null,
      password: null,
      format: "dd/MM/yyyy",
      selectedDOB: null,
    };
  },
  methods: {
   
    async createNewModerator() {
      const dd = this.selectedDOB.getDate();
      const mm = this.selectedDOB.getMonth() + 1;
      const yy = this.selectedDOB.getFullYear();
      const d = yy + "/" + dd + "/"  + mm
      const mailformat = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
      if (!this.email.match(mailformat)) {
        await this.$notify({
          type: "danger",
          message: "Entered mail address is not valid",
        });
        return;
      }
      try {
        axios.defaults.headers.common["Authorization"] = `Bearer ${localStorage.token}`;
        await axios.post(CREATE_EMPLOYEE, {
          full_name: this.fullname,
          email: this.email,
          phone_number: this.phonenumber,
          role_id: "4",
          user_name: this.username,
          user_password: this.password,
          dob: d,
        });
        this.$router.push("moderatorlist");
      } catch (error) {
        // console.log("looxi owr day", error);
        await this.$notify({
          type: "danger",
          message: error.response.data.meta.message,
        });
        return;
      }
    },
  },
};
</script>
<style>
#preview {
  display: flex;
  justify-content: center;
  align-items: center;
}
#preview img {
  max-width: 50%;
  max-height: 500px;
}
</style>
