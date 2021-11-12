<template>
  <div class="flexContainer">
    <!-- multiple tab user -->
    <div id="tabs" class="container col-md-7 card">
      <div class="tabs">
        <a
          v-on:click="activetab = 1"
          v-bind:class="[activetab === 1 ? 'active' : '']"
          >Moderator</a
        >
        <a
          v-on:click="activetab = 2"
          v-bind:class="[activetab === 2 ? 'active' : '']"
          >Restaurant owner</a
        >
        <a
          v-on:click="activetab = 3"
          v-bind:class="[activetab === 3 ? 'active' : '']"
          >Cashier</a
        >
        <a
          v-on:click="activetab = 4"
          v-bind:class="[activetab === 4 ? 'active' : '']"
          >Applicaion user</a
        >
      </div>
      <!-- tab content -->
      <div>
        <div v-if="activetab === 1" class="tabcontent moderatortab">
          <table-user-admin 
            v-bind:id="id"
            v-bind:listTile="listModerator.name"
            v-bind:list="listModerator.listusers"
            v-bind:tableField="tableField"
            v-on:deleteRequest="confirmDelete"
            v-on:viewDetail="viewUserDetail"
          />
          
        </div>
        <div v-if="activetab === 2" class="tabcontent restaurantownertab">
          <table-user-admin 
            v-bind:id="id"
            v-bind:listTile="listRetaurantOwner.name"
            v-bind:list="listRetaurantOwner.listusers"
            v-bind:tableField="tableField"
            v-on:deleteRequest="confirmDelete"
            v-on:viewDetail="viewUserDetail"
          />
        </div>
        <div v-if="activetab === 3" class="tabcontent cashiertab">
           <table-user-admin 
            v-bind:id="id"
            v-bind:listTile="listCashier.name"
            v-bind:list="listCashier.listusers"
            v-bind:tableField="tableField"
            v-on:deleteRequest="confirmDelete"
            v-on:viewDetail="viewUserDetail"
          />
        </div>
        <div v-if="activetab === 4" class="tabcontent appusertab">
          <table-user-admin 
            v-bind:id="id"
            v-bind:listTile="listAppUser.name"
            v-bind:list="listAppUser.listusers"
            v-bind:tableField="tableField"
            v-on:deleteRequest="confirmDelete"
            v-on:viewDetail="viewUserDetail"
          />
        </div>
      </div>
    </div>
    <modal
                :show.sync="modals.mini"
                class="modal-primary"
                :show-close="false"
                headerClasses="justify-content-center"
                type="mini"
              >
                <div slot="header" class="modal-profile ml-auto mr-auto">
                  <i class="fa fa-exclamation"></i>
                </div>
                <p>Delete this moderator?</p>
                <template slot="footer">
                  <div class="left-side">
                    <p-button type="default" link @click="modals.mini = false"
                      >No</p-button
                    >
                  </div>
                  <div class="divider"></div>
                  <div class="right-side">
                    <p-button type="default" link @click="deleteUser"
                      >Yes</p-button
                    >
                  </div>
                </template>
              </modal>
    <div class="col-md-4 card roles">
      <h4 class="title">Systems roles</h4>
      <el-table :data="systemsRoles">
        <el-table-column prop="role_id" label="ID"> </el-table-column>
        <el-table-column
          min-width="150"
          prop="role_name"
          label="Name"
        ></el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import { Table, TableColumn } from "element-ui";
import {
  GET_SYSTEMS_USER,
  DELETE_USER,
  GET_ROLES,
} from "../../../../../src/constants/api.js";
import { Modal } from "src/components/UIComponents";
import TableUserAdmin from "../Components/TableUserAdmin.vue";
import axios from "axios";
Vue.use(Table);
Vue.use(TableColumn);

export default {
  components: {
    Modal,
    TableUserAdmin
  },
  data() {
    return {
      systemsRoles: null,
      systemsUsers: null,
      modals: {
        mini: false,
      },
      selectedUserID: null,
      activetab: 1,
      listModerator: {
        name: "moderator",
        listusers: null,
      },
      listCashier:  {
        name: "cashier",
        listusers: null,
      },
      listRetaurantOwner: {
        name: "restaurant owner",
        listusers: null,
      },
      listAppUser: {
        name: "restaurant owner",
        listusers: null,
      },
      tableField : [
        {lable : "Name", property : "full_name"},
        {lable : "Phone Number", property : "phone_number"},
        ],
      id : "user_id"
    };
  },
  methods: {
    confirmDelete(e) {
      console.log("idđ" , e.choosenID)
      this.modals.mini = true;
      this.selectedUserID = e.choosenID;
    },
    viewUserDetail(e) {
      console.log("viewUSerrrrrrr")
      this.$router.push({
        name: "managemoderator",
        params: { user_id: e.choosenID },
      });
    },
    async getListRoles() {
      try {
        const response = await axios.get(GET_ROLES);
        this.systemsRoles = response.data.data;
      } catch (error) {
        console.log(error);
        // await this.$notify({
        //   type: "danger",
        //   message: "Server is not responsed",
        // });
        // return;
      }
    },
    async getListUsers() {
      try {
        axios.defaults.headers.common[
          "Authorization"
        ] = `Bearer ${localStorage.token}`;
        const response = await axios.get(GET_SYSTEMS_USER);
        this.systemsUsers = response.data.data;
        console.log("systemsusers ", this.systemsUsers);
        this.listModerator.listusers = this.systemsUsers.filter(
          (systemsUsers) => systemsUsers.role_name === "moderator"
        );
        console.log("moderator list : " , this.listModerator.listusers),
        this.listCashier.listusers = this.systemsUsers.filter(
          (systemsUsers) => systemsUsers.role_name === "cashier"
        );
        this.listAppUser.listusers = this.systemsUsers.filter(
          (systemsUsers) => systemsUsers.role_name === "user"
        );
        this.listRetaurantOwner.listusers = this.systemsUsers.filter(
          (systemsUsers) => systemsUsers.role_name === "restaurant owner"
        );
      } catch (error) {
        console.log(error);
        // await this.$notify({
        //   type: "danger",
        //   message: "Server is not responsed",
        // });
        // return;
      }
    },
    async deleteUser() {
      this.modals.mini = false;
      try {
        await axios.delete(DELETE_USER, {
          params: { user_id: this.selectedUserID },
        });
        this.getListUsers();
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
    this.getListUsers();
    this.getListRoles();
  },
};
</script>
<style scoped>
.flexContainer {
  display: flex;
  margin-left: -60px;
}
.roles {
  height: fit-content;
}
/* Import Google Font */
@import url(https://fonts.googleapis.com/css?family=Nunito+Sans);

/* STYLING */
/* .container {  
   
    min-width: 420px;
    
    font-family: "Nunito Sans", Arial, Helvetica, sans-serif;
    color: #888;
} */

/* Style the tabs */
.tabs {
  overflow: hidden;
  margin-left: 20px;
  margin-bottom: -2px;
}

.tabs ul {
  list-style-type: none;
  /* margin-left: 20px; */
}

.tabs a {
  float: left;
  cursor: pointer;
  padding: 12px 24px;
  transition: background-color 0.2s;
  border: 1px solid #ccc;
  border-right: none;
  background-color: #f1f1f1;
  border-radius: 10px 10px 0 0;
  font-weight: bold;
}
.tabs a:last-child {
  border-right: 1px solid #ccc;
}

/* Change background color of tabs on hover */
.tabs a:hover {
  background-color: #aaa;
  color: #fff;
}

/* Styling for active tab */
.tabs a.active {
  background-color: #fff;
  color: #484848;
  border-bottom: 2px solid #fff;
  cursor: default;
}

/* Style the tab content */
.tabcontent {
  /* padding: 30px; */
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 3px 3px 6px #e1e1e1;
}
</style>
