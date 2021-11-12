<template>
  <div class="card">
    <div class="card-header">
      <h4 class="title"><span v-if="!view">Edit</span> Cashiers Profile</h4>
    </div>
    <div class="card-body">
      <form>
        <div class="row">
          <div class="col-md-7">
            <fg-input
              type="text"
              label="ID"
              :disabled="true"
              placeholder="id"
              v-model="userID"
            >
            </fg-input>

            <fg-input
              :disabled="view"
              type="text"
              label="Name"
              placeholder="name"
              v-model="user.full_name"
            >
            </fg-input>
            <fg-input
              :disabled="view"
              type="text"
              label="DOB"
              placeholder="dob"
              v-model="user.dob"
            >
            </fg-input>

            <fg-input
              :disabled="view"
              type="Text"
              label="Phone"
              placeholder="phone"
              v-model="user.phone_number"
            >
            </fg-input>

            <fg-input
              :disabled="view"
              type="text"
              label="Email"
              placeholder="email"
              v-model="user.email"
            >
            </fg-input>

            <div class="text-center">
              <p-button v-on:click="EnableEdit" v-if="view" type="info"
                >Edit Profile</p-button
              >
              <button
                v-if="!view"
                type="submit"
                class="btn btn-info btn-fill btn-wd"
                @click.prevent="updateProfile"
              >
                Update Profile
              </button>
              <p-button v-if="view" v-on:click="deleteUser" type="danger"
                >Delete Cashier</p-button
              >
            </div>
          </div>
        </div>

        <div class="clearfix"></div>
      </form>
    </div>
  </div>
</template>
<script>
import { GET_CASHIER,DELETE_USER } from "../../../../../src/constants/api.js";
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
      view: true,
    };
  },
  methods: {
    async updateProfile() {
      try {
        const response = await axios.post(
          GET_CASHIER,
          {
            params: { user_id: this.userID },
          },
          {
            dob: this.user.dob,
            email: this.user.email,
            full_name: this.user.full_name,
            phone_number: this.user.phone_number,
          }
        );
      } catch (error) {
        console.log(error);
        // await this.$notify({
        //   type: "danger",
        //   message: "Server is not responsed",
        // });
        // return;
      }
      this.$router.back();
    },
    EnableEdit() {
      this.view = false;
    },
    async deleteUser() {
      try {
        await axios.delete(DELETE_USER, {
          params: { user_id: this.userID },
        });
        
      } catch (error) {
        console.log(error);
        // await this.$notify({
        //   type: "danger",
        //   message: "Server is not responsed",
        // });
        // return;
      }
      this.$router.back();
    },
    async getDetail() {
      try {
        const response = await axios.get(GET_CASHIER, {
          params: { user_id: this.userID },
        });
        const detail = response.data.data;
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
