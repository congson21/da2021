<template>
  <div class="">
    <div class="card-header">
      <h4 class="card-title">List of {{ listTile }}</h4>
    </div>
    <div class="col-sm-12">
      <div class="pull-right">
        <fg-input
          class="input-sm"
          placeholder="Search"
          addon-right-icon="nc-icon nc-zoom-split"
        >
        </fg-input>
      </div>
    </div>
   
    <div class="card-body">
            <div class="card-body table-responsive table-full-width">
              <el-table :data="list">
                <el-table-column type="index"></el-table-column>
                <el-table-column v-for="field in tableField" :key="field.lable" :label="field.lable" :property="field.property"></el-table-column>
                <el-table-column
                  fixed="right"
                  class-name="td-actions"
                  label="Actions"
                >
                <template slot-scope="scope">
                    <p-button
                      type="success"
                      size="sm"
                      icon
                      v-on:click="viewDetail(list[scope.$index][id])"
                    >
                      <i class="fa fa-edit"></i>
                    </p-button>
                    <p-button
                      type="danger"
                      size="sm"
                      icon
                      v-on:click="deleteItem(list[scope.$index][id])"
                    >
                      <i class="fa fa-times"></i>
                    </p-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>
  </div>
  </div>
</template>
<script>
export default {
  name: "table-user-admin",
  props: {
    id: {
      type: String,
      description : "Show list name customed in father component"
    },
    listTile:{
      type: String,
      description : "Show list name customed in father component"
    } ,
    list: {
      type: Array,
      description : "List data sent from father component"
    },
    tableField: {
      type: Array,
      description : "Custom table field after userwish data sent from father component"
    }
  },
  
  data() {
    return {
      
    };
  },
  methods: {
    // Send delete request to father component
    deleteItem(ID){
      var data = {
        choosenID : ID
      }
      this.$emit('deleteRequest', data)
    },
    // Call View Detail page in father component
    viewDetail(ID){
      console.log("viewww")
      var data = {
        choosenID : ID
      }
      this.$emit("viewDetail", data)
    }
  },
};
</script>
<style scoped>
</style>