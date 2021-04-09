<template>
  <div>
    <CCard>
      <CCardHeader>
        <h3>List Orders</h3>
      </CCardHeader>
      <CCardBody>
        <!-- <CButton color="primary" class="m-2 btn_add">
          <nuxt-link to="/task/add" class="text-white d-block w-100">
            + Add</nuxt-link
          >
        </CButton> -->
        <CDataTable
          :items="dataOrders"
          :fields="fields"
          items-per-page-select
          :items-per-page="5"
          pagination
        >
          <template #customer_id="{ item }">
            <td>
              {{ item.customer.name }}
            </td>
          </template>

          <template #status="{ item }">
            <td>
              {{ filter(item.status, STATUS) }}
            </td>
          </template>

          <template #method="{ item }">
            <td class="py-2">
              <CButton color="success">
                <nuxt-link :to="`/task/${item.id}`">
                  <CIcon :content="$options.freeSet.cilPencil" />
                </nuxt-link>
              </CButton>
              <CButton color="danger" @click="deleteTask(item.id)">
                <CIcon :content="$options.freeSet.cilTrash" />
              </CButton>
            </td>
          </template>
          <!-- ============================================================================================================== -->
          <template #show_details="{ item, index }">
            <td class="py-2">
              <CButton
                color="primary"
                variant="outline"
                square
                size="sm"
                @click="toggleDetails(item, index)"
              >
                {{ Boolean(item._toggled) ? "Hide" : "Detail" }}
              </CButton>
            </td>
          </template>

          <template #details="{ item }">
            <CCollapse
              :show="Boolean(item._toggled)"
              :duration="collapseDuration"
            >
              <CCard>
                <CCardHeader color="dark" > <h6 class="text-white">Order Detail</h6> </CCardHeader>
                <CCardBody color="secondary" class="text-dark">
                  <CRow class="fw-bold">
                    <CCol sm="8">
                      <CRow class="mb-3">
                        <CCol sm="4">
                          Name Buyer : {{ item.customer.name }}
                        </CCol>
                        <CCol sm="4"> Addres : {{ item.address }} </CCol>
                        <CCol sm="4"> Phone : {{ item.phone }} </CCol>
                      </CRow>
                      <CRow>
                        <CCol sm="4">
                          Status : {{ filter(item.status, STATUS) }}
                        </CCol>
                        <CCol sm="4"> Order Date : {{ item.date }} </CCol>
                        <CCol sm="4"> Receiver : {{ item.name }} </CCol>
                      </CRow>
                    </CCol>
                    <CCol sm="4">
                      <CRow>
                        <CCol sm="12">
                          Total : {{ formatOriginalPrice(item.total_price) }}
                        </CCol>
                      </CRow>
                    </CCol>
                  </CRow>

                  <CDataTable
                    :items="details"
                    :fields="fields1"
                    :items-per-page="5"
                    pagination
                    
                    class="mt-5"
                  >
                    <template #thumb="{ item }">
                      <td class="thumb-product">
                        <img
                          v-bind:src="
                            'http://localhost:8000/' + item.product.thumb
                          "
                          class="img-fluid w-75"
                        />
                      </td>
                    </template>

                    <template #product="{ item }">
                      <td>
                        {{ item.product.name }}
                      </td>
                    </template>
                     <template #price="{ item }">
                      <td>
                        {{ formatOriginalPrice( item.product.price) }}
                      </td>
                    </template>
                  </CDataTable>
                </CCardBody>
              </CCard>
            </CCollapse>
          </template>
        </CDataTable>
      </CCardBody>
    </CCard>
  </div>
</template>
<script>
import axios from "axios";
import { freeSet } from "@coreui/icons";
import { URL_API } from "~/constant/constant";
import { DATA_STATUS } from "@/constant/constant";
import swal from "sweetalert2";
const fields1 = [
  { key: "id", label: "Id" },
  { key: "thumb", label: "Thumb" },
  { key: "product", label: "Product" },
  { key: "price", label: "Price" },
  { key: "qty", label: "Amount" },
  { key: "sub_total", label: "Sub Total" },
];
const fields = [
  { key: "id", label: "Id" },
  { key: "customer_id", label: "Name" },
  { key: "address", label: "Address" },
  { key: "phone", label: "Phone" },
  { key: "status", label: "Status" },
  { key: "method", label: "Method" },
  {
    key: "show_details",
    label: "",
    _style: "width:1%",
    sorter: false,
    filter: false,
  },
];
export default {
  name: "ListOrder",
  freeSet,
  props: {
    dataOrders: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      fields: fields,
      fields1: fields1,
      STATUS: DATA_STATUS,
      details: [],
      collapseDuration: 0,
      dataOrders: this.dataOrders.map((item, id) => {
        return { item, id };
      }),
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

    /**
     * show detail order
     */
    toggleDetails(item, index) {
      console.log(item.id);
      this.$set(this.dataOrders[index], "_toggled", !item._toggled);
      this.collapseDuration = 300;
      this.$nextTick(() => {
        this.collapseDuration = 0;
        //   this.getDataById(item.id);
      });
    },

    /**
     * delete order
     */
    deleteOrder(id) {
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
            axios.delete(URL_API + "order/" + id).then((res) => {
              this.$emit("getListOrders", this.dataOrders);
            });
            swal.fire("Deleted!", "Your file has been deleted.", "success");
          }
        });
    },

    /**
     * get name status
     */
    filter(id, array) {
      return array.find((value, index) => {
        if (index === id) {
          return value;
        }
      });
    },

    getOrderDetail() {
      axios.get(URL_API + "order-detail").then((res) => {
        this.details = res.data.data;
        // this.page = res.data
        console.log(this.details);
      });
    },
  },
  mounted() {
    this.getOrderDetail();
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
.thumb-product {
  width: 150px;
}
</style>