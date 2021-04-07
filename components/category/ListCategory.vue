<template>
  <div >
    <CCard>
      <CCardHeader>
        <h3>List Categories</h3>
      </CCardHeader>
      <CCardBody>
        <CButton color="primary" class="m-2 btn_add">
          <nuxt-link to="/category/add" class="text-white d-block"> + Add</nuxt-link>
        </CButton>

        <CDataTable
          :items="dataCategory"
          :fields="fields"
          items-per-page-select
          :items-per-page="5"
          hover
          pagination
        >
          <template #method="{ item }">
            <td class="py-2">
              <CButton color="success">
                <nuxt-link :to="`/category/${item.id}`">
                  <CIcon :content="$options.freeSet.cilPencil" />
                </nuxt-link>
              </CButton>
              <CButton color="danger" @click="deleteCategory(item.id)">
                <CIcon :content="$options.freeSet.cilTrash" />
              </CButton>
            </td>
          </template>
        </CDataTable>
      </CCardBody>
    </CCard>
  </div>
</template>
<script>
import axios from "axios";
import { freeSet } from "@coreui/icons";
import swal from "sweetalert2";

const fields = [
  { key: "id", label: "Id", _style: "min-width:100px" },
  { key: "name", label: "Name", _style: "min-width:150px" },
  { key: "des", label: "Description", _style: "min-width:100px;" },
  { key: "method", label: "Method", _style: "min-width:100px;" },
];

export default {
  name: "ListCategory",
  freeSet,
  props: {
    dataCategory: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      fields: fields,
    };
  },
  methods: {
    /**
     * delete category
     */
    deleteCategory(id) {
      swal
        .fire({
          title: "Are you sure?",
          text: "You won't be able to revert this!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it!",
        })
        .then((result) => {
          if (result.isConfirmed) {
            axios
              .delete("http://localhost:8000/api/category/" + id)
              .then((res) => {
                this.$emit("getListCategory", this.dataCategory);
              });
            swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },
  },
};
</script>
<style>
.btn_add{
  width: 100px;
  position: absolute;
  top: 20px;
}
.card-body{
  position: relative;
}
</style>
