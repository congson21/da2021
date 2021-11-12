<template>
  <div class="row">
    <div class="col-md-12">
      <div class="card">
        <div
          class="card-header d-flex justify-content-between align-items-center"
        >
          <h4 class="title">List of product</h4>
        </div>
        <div class="card-body">
          <div class="card-body table-full-width">
            <el-table :data="tableData">
              <el-table-column
                label="ID"
                width="86px"
                type="index"
              ></el-table-column>
              <el-table-column
                label="Name"
                property="product_name"
                width="200px"
              ></el-table-column>
              <el-table-column
                label="Price"
                property="price"
                width="150px"
              ></el-table-column>
              <el-table-column
                label="Unit"
                property="product_type"
                width="100px"
              ></el-table-column>
              <el-table-column
                width="120px"
                label="Category"
                property="categories_name"
              ></el-table-column>
              <el-table-column
                label="Status"
                property="is_out_of_stock"
                width="160px"
                align="center"
              >
                <template slot-scope="scope">
                <div type="success">
                  {{ scope.row.is_out_of_stock  ? "Is out of stock": "Available" ||  status  }}
                  
                </div>
               </template>
              </el-table-column>

              <!-- <el-table-column align="right" property="salary">
                <p-button type="info" v-on:click="viewDetailProduct"
                  >View</p-button
                ></el-table-column -->

              <el-table-column label="" property="" width="200px">
                <template slot-scope="scope">
                  <p-button
                    type="info"
                    v-on:click="
                      viewDetailProduct(tableData[scope.$index].product_id)
                    "
                    >View</p-button
                  ></template
                ></el-table-column
              >
              >
            </el-table>
          </div>
          <export-excel
            class="btn btn-info"
            :data="tableData"
            worksheet="My Worksheet"
            name="ListProduct.xls"
          >
            Download Excel
          </export-excel>

          <el-button class="btn btn-info mr-3" @click="dialogVisible = true"
            >Add New Product</el-button
          >
          <form @submit.prevent="addNew">
            <el-dialog :visible.sync="dialogVisible" width="60%">
              <span slot="title" class="h3"
                ><i class="el-icon-circle-plus mr-2"></i>Add New Product</span
              >

              <div class="row">
                <div class="col-sm-7 ml-3">
                  <div class="row">
                    <div class="col-sm-12 mb-2">
                      <fg-input
                        type="text"
                        label="Product code:"
                        placeholder="Enter code"
                        v-model="product_code"
                      >
                      </fg-input>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-sm-12 mb-2">
                      <fg-input
                        type="text"
                        label="Product name:"
                        placeholder="Enter name"
                        v-model="product_name"
                      >
                      </fg-input>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-sm-12 mb-2">
                      <fg-input
                        type="text"
                        label="Price:"
                        placeholder="Enter price"
                        v-model="product_price"
                      >
                      </fg-input>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-sm-12 mb-2">
                      <fg-input
                        type="text"
                        label="Unit:"
                        placeholder="Enter unit"
                        v-model="product_type"
                      >
                      </fg-input>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-sm-12 mb-3">
                      <label for="category">Category:</label>
                      <select
                        id="category"
                        class="form-control p-2"
                        v-model="product_category"
                      >
                        <option
                          v-for="cateItem in categoryItem"
                          v-bind:key="cateItem.category"
                          v-bind:value="{
                            id: cateItem.id,
                            text: cateItem.category,
                          }"
                          :selected="cateItem.category == 'Rice'"
                        >
                          {{ cateItem.category }}
                        </option>
                      </select>
                    </div>
                  </div>
                  <!-- <div class="row">
                    <div class="col-sm-12 mb-3">
                      <label for="status">Status:</label>
                      <select
                        id="status"
                        class="form-control p-2"
                        v-model="product_status"
                      >
                        <option
                          v-for="item in statusItem"
                          v-bind:key="item"
                          :selected="item == 'Available'"
                        >
                          {{ item }}
                        </option>
                      </select>
                    </div>
                  </div> -->
                </div>
                <div class="col-sm-4">
                  <div
                    id="add-product"
                    class="text-center img-container-add-product"
                  >
                    <div class="fake-image"></div>
                    <img v-if="url" :src="url" class="img-add-product" />
                  </div>
                  <div class="text-center">
                    <input
                      type="file"
                      @change="onSelected"
                      class="btn-open-img"
                      id="file-input"
                    />
                    <button
                      class="btn btn-info"
                      type="file"
                      @click.prevent="openImage"
                    >
                      Choose Image
                    </button>
                  </div>
                  <div class="row">
                    <div class="col-sm-12 mb-2 mt">
                      <textarea
                        class="des"
                        type="text"
                        placeholder="Description"
                        v-model="product_description"
                      >
                      </textarea>
                    </div>
                  </div>
                </div>
              </div>

              <span slot="footer" class="dialog-footer">
                <el-button type="button" @click="dialogVisible = false"
                  >Close</el-button
                >
                <el-button
                  type="submit"
                  native-type="submit"
                  class="btn btn-info"
                  @click="dialogVisible = false"
                  >Save</el-button
                >
              </span>
            </el-dialog>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import excel from "vue-excel-export";
import { Table, TableColumn, Button, Dialog } from "element-ui";
import axios from "axios";
import { CREATE_PRODUCT } from "../../../../../src/constants/api.js";
import { GET_LIST_TABLE_PRODUCT } from "../../../../../src/constants/api.js";
import { GET_PRODUCT_DETAIL } from "../../../../../src/constants/api.js";
import FormData from "form-data";
import { BFormTextarea } from "bootstrap-vue";
Vue.component("b-form-textarea", BFormTextarea);

Vue.use(Table);
Vue.use(TableColumn);
Vue.use(Button);
Vue.use(Dialog);
Vue.use(excel);

export default {
  data() {
    return {
      url: null,
      img: null,
      products: [],
      product_name: "",
      product_price: "",
      product_category: "",
      product_status: "",
      product_code: "",
      product_type: "",
      product_description: "",
      token: localStorage.token || "",
      dialogVisible: false,
      categoryItem: [
        { id: 1, category: "Rice" },
        { id: 2, category: "Food" },
        { id: 3, category: "Drink" },
        { id: 4, category: "Noddle" },
        { id: 5, category: "Snacks" },
        { id: 6, category: "Cake" },
      ],
      statusItem: ["Available", "Out of stock"],
      tableData: [],
    };
  },

  created() {
    this.getProductTable();
  },

  methods: {
    openImage() {
      document.querySelector(".btn-open-img").click();
    },
    onSelected(event) {
      this.product_image = event.srcElement.files[0];
      this.url = URL.createObjectURL(this.product_image);
      document.querySelector(".fake-image").classList.add("hide-fake-img");
    },
    // tableRowStyle({ row, rowIndex }) {
    //   if (row.status == "Available") {
    //     return "color: green";
    //   } else {
    //     return "color: red";
    //   }
    // },
    // viewDetailProduct() {
    //   this.$router.push("productmanagement");
    // },
    viewDetailProduct(productID) {
      this.$router.push({
        name: "productmanagement",
        params: { product_id: productID },
      });
    },
    //
    openImageDialog() {
      const imageFile = document.querySelector("#image");
      imageFile.click();
    },
    onFileSelected(e) {
      const imageInput = document.querySelector("#imageInput");
      imageInput.value = e.target.value;
    },

    addNew() {
      //  const formData = new FormData();
      //  product_image = document.querySelector('#file');
      //  formData.append("image", product_image.files[0]);
      axios.defaults.headers.common["Authorization"] = `Bearer ${this.token}`;
      axios
        // .post(CREATE_PRODUCT, formData,  {
        .post(CREATE_PRODUCT, {
          product_name: this.product_name,
          price: this.product_price,
          category_id: this.product_category.id,
          product_code: this.product_code,
          description: this.product_description,
          product_type: this.product_type,
        })
        .then((res) => {
          this.$notify({
            message: "Add Successfull!",
            type: "success",
          });
        })
        .catch((err) => {
          this.$notify({
            message: "Add failed! Try again later!",
            type: "danger",
          });
        });
    },

    getProductTable() {
      axios.defaults.headers.common["Authorization"] = `Bearer ${this.token}`;
      axios
        .get(GET_LIST_TABLE_PRODUCT)
        .then((res) => {
          this.tableData = res.data.data
          console.log(this.tableData);
        })
        .catch((err) => {
          this.$notify({
            message: "No thing!",
            type: "danger",
          });
        });
    },
  },
};
</script>
<style>
.el-dialog__body {
  padding: 10px 20px;
}
.card label {
  font-size: 1rem !important;
  margin-bottom: 5px !important;
  color: #333 !important;
}
#imageInput {
  width: 90%;
}
/* .el-table__body .el-table_1_column_5 .cell {
  border-radius: 5px;
  color: green;
  display: inline;
  text-align: center;
} */
#add-product {
  display: flex;
  justify-content: center;
  align-items: center;
}
#add-product img {
  max-width: 70%;
  max-height: 400px;
}
.btn-open-img {
  display: none;
}
.fake-image {
  width: 150px;
  height: 150px;
  border: 0.5px ridge rgb(219, 216, 216);
}

.img-add-product {
  object-fit: contain;
}

.img-container-add-product {
  width: 90%;
  height: 228px;
}

.hide-fake-img {
  display: none;
}

.input-form {
  border: 1px solid black;
}

.mt {
  margin-top: 5px;
}

.des {
  width: 110%;
  height: 122px;
  outline: rgb(240, 240, 240) solid 0.1px;
  border: 1px solid rgb(187, 187, 187);
}
</style>
