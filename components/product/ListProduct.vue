<template>
  <div>
    <CCard>
      <CCardHeader>
        <h3>List Products</h3>
      </CCardHeader>
      <CCardBody>
        <CButton color="primary" class="m-2 btn_add">
          <nuxt-link to="/product/add" class="text-white d-block">
            + Add</nuxt-link
          >
        </CButton>

        <CDataTable
          :items="dataProducts"
          :fields="fields"
          items-per-page-select
          :items-per-page="5"
          hover
          pagination
        >
          <template #thumb="{ item }">
            <td class="thumb">
           
              <img
               
                v-bind:src="'http://localhost:8000/' + item.thumb"
                class="img-fluid w-75"
              />
            </td>
          </template>
          <template #category_id="{ item }">
            <td>
              {{ item.category.name }}
            </td>
          </template>
          <template #price="{ item }">
            <td>
              {{ formatOriginalPrice(item.price) }}
            </td>
          </template>
          <template #method="{ item }">
            <td class="py-2">
              <CButton color="success">
                <nuxt-link :to="`/product/${item.id}`">
                  <CIcon :content="$options.freeSet.cilPencil" />
                </nuxt-link>
              </CButton>
              <CButton color="danger" @click="deleteProduct(item.id)">
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
import { URL_API } from "~/constant/constant";

const fields = [
  { key: "id", label: "Id" },
  { key: "thumb", label: "Thumb" },
  { key: "name", label: "Name" },
  { key: "category_id", label: "Category" },
  { key: "price", label: "Price" },
  { key: "discount", label: "Discount" },
  { key: "qty", label: "Amount" },
  { key: "des", label: "Description" },
  { key: "method", label: "Method" },
];

export default {
  name: "ListProduct",
  freeSet,
  props: {
    dataProducts: {
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
     * format price
     */
    formatOriginalPrice(number) {
      return new Intl.NumberFormat("de-DE", {
        style: "currency",
        currency: "VND",
      }).format(number);
    },

     getImageBlob (image_url) {
     return axios.get(image_url).then(response => window.URL.createObjectURL(response.data))
  },
     
    /**
     * delete product
     */
    deleteProduct(id) {
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
              .delete(URL_API +'product/' + id)
              .then((res) => {
                this.$emit("getListProducts", this.dataProducts);
              });
            swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },
  },
};
</script>
<style>
.btn_add {
  width: 100px;
  position: absolute;
  top: 20px;
}
.card-body {
  position: relative;
}
.thumb {
  width: 100px;
}
</style>
