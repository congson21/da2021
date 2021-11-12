edit product

<template>
  <div class="card">
    <div class="card-header">
      <h4 class="title"><span v-if="!view">Edit</span> Product Details</h4>
    </div>
    <div class="card-body">
      <form>
        <div class="row">
          <div class="col-md-4">
            <img
              src="/static/img/mike.jpg"
              alt="W3Schools.com"
              width="100"
              height="132"
            />
          </div>

          <div class="col-md-7">
            <div class="row">
              <!-- product code -->
              <div class="col-md-6">
                <fg-input
                  :disabled="view"
                  type="text"
                  label="Code"
                  placeholder="code"
                  v-model="product.product_code"
                >
                </fg-input>
              </div>
              <!-- product name -->
              <div class="col-md-6">
                <fg-input
                  :disabled="view"
                  type="Text"
                  label="Name"
                  placeholder="name"
                  v-model="product.product_name"
                >
                </fg-input>
              </div>
            </div>

            <div class="row">
              <div class="col-md-6">
                <fg-input
                  :disabled="view"
                  type="Text"
                  label="Price"
                  placeholder="price"
                  v-model="product.product_price"
                >
                </fg-input>
              </div>
              <div class="col-md-6">
                <fg-input
                  :disabled="view"
                  type="Text"
                  label="Unit"
                  placeholder="unit"
                  v-model="product.product_unit"
                >
                </fg-input>
              </div>
            </div>

            <div class="row">
              <div class="col-md-6">
                <label for="category">Category</label>
                <select :disabled="view" id="category" class="form-control">
                  <option
                    v-for="item in categories"
                    v-bind:key="item"
                    :selected="item == product.product_category"
                  >
                    {{ item }}
                  </option>
                </select>
              </div>
              <div class="col-md-6">
                <label for="status">Status</label>
                <select :disabled="view" id="status" class="form-control">
                  <option
                    v-for="item in status"
                    v-bind:key="item"
                    :selected="item == product.product_status"
                  >
                    {{ item }}
                  </option>
                </select>
              </div>
            </div>
            <br />
            <div>
              <label for="description">Description</label>

              <textarea
                :disabled="view"
                class="des"
                type="text"
                placeholder="Description"
                v-model="product.product_description"
              >
              </textarea>
            </div>
            <br />
            <div class="text-center">
              <p-button v-on:click="EnableEdit" v-if="view" type="info"
                >Edit Product</p-button
              >
              <button
                v-if="!view"
                type="submit"
                class="btn btn-info btn-fill btn-wd"
                @click.prevent="updateProduct"
              >
                Update Product
              </button>
              <p-button v-if="view" v-on:click="deleteProduct" type="danger"
                >Delete Product</p-button
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
import Vue from "vue";
import {
  GET_PRODUCT_DETAIL,
  DELETE_PRODUCT,
  UPDATE_PRODUCT,
} from "../../../../../src/constants/api.js";
import axios from "axios";
export default {
  data() {
    return {
      product: {
        product_code: "",
        product_name: "",
        product_unit: "",
        product_description: "",
        product_price: "",
        product_category: "",
        product_status: "",
      },
      categories: ["Rice", "Food", "Drink", "Noddle", "Snacks", "Cake"],
      status: ["Is out of stock", "Available"],
      productID: this.$route.params.product_id,
      view: true,
      token: localStorage.token || "",
    };
  },
  created() {
    this.getProductDetail();
  },
  methods: {
    updateProduct() {
      axios.defaults.headers.common["Authorization"] = `Bearer ${this.token}`;
      axios
        .put(UPDATE_PRODUCT + this.productID, {
          product_code: this.product.product_code,
          name: this.product.product_name,
          product_type: this.product.product_unit,
          price: this.product.product_price,
          description: this.product.product_description,
          // categories: this.product.product_category,
          product_type: this.product.product_unit,
          // is_out_of_stock: this.product.product_status,
        })
        .then((res) => {
          this.$notify({
            message: "Update Successfull!",
            type: "success",
          });
          this.$router.go(-1);
        })
        .catch((err) => {
          this.$notify({
            message: "Update Fail!",
            type: "danger",
          });
        });
    },

    EnableEdit() {
      this.view = false;
    },

    // delete product
    deleteProduct() {
      axios.defaults.headers.common["Authorization"] = `Bearer ${this.token}`;
      axios
        .delete(DELETE_PRODUCT + this.productID)
        .then((res) => {
          this.$notify({
            message: "Delete Successfull!",
            type: "success",
          });
          this.$router.go(-1);
        })
        .catch((err) => {
          this.$notify({
            message: "Delete Fail!",
            type: "danger",
          });
        });
    },

    // detail product
    async getProductDetail() {
      try {
        const response = await axios.get(GET_PRODUCT_DETAIL, {
          params: { product_id: this.productID },
        });

        const detail = response.data.data;
        this.product.product_code = detail.product_code;
        this.product.product_name = detail.name;
        this.product.product_unit = detail.product_type;
        this.product.product_price = detail.price;
        this.product.product_category = detail.categories;
        this.product.productStatus = detail.is_out_of_stock
          ? "Is out of stock"
          : "Available";
        this.product.product_description = detail.description;
      } catch (error) {
        await this.$notify({
          type: "danger",
          message: "Error",
        });
        return;
      }
    },
  },
};
</script>
<style>
</style>
