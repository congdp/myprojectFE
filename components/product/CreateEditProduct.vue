<template lang="">
  <div>
      <div>
    <CCard>
      <CCardHeader >
        <h3>{{title}}</h3>
      </CCardHeader>
      <CCardBody>
        <CRow>
          <CCol sm="6">
            <label for="source">Category</label>
               <select id="id_project" v-model="form.category_id" class="form-control">

              <option
                v-for="(item, index) in dataCategory"
                :key="index"
                :value="item.id"
              >
                {{item.name }}
              </option>
            </select>
          </CCol>
          <CCol sm="6">
            <CInput label="Name" placeholder="Name" v-model="form.name">
            </CInput>
          </CCol>
        </CRow>

         <CRow>
          <CCol sm="4">
            <CInput label="Amount" type="number" placeholder="Amount" v-model="form.qty">
            </CInput>
          </CCol>
     
          <CCol sm="4">
            <CInput label="Price" type="number" placeholder="Price" v-model="form.price">
            </CInput>
          </CCol>

           <CCol sm="4">
    
             <CInput label="Discount" type="number" placeholder="Discount" v-model="form.discount">
            </CInput>
          </CCol>
        </CRow>
      
        <CRow>
          <CCol sm="12">
            <CTextarea label="Description" rows="5" v-model="form.des">
            </CTextarea>
          </CCol>
        </CRow>
        <CRow>
          <CCol sm="4">
             <CInput label="Thumb" type="file" v-model="form.thumb">
            </CInput>
          </CCol>
        </CRow>
         <CRow>
          <ul v-if="errors.length > 0" class="alert alert-danger">
          <li v-for="error in errors" :key="error">{{ error }}</li>
         </ul>
        </CRow>
      </CCardBody>
      <CCardFooter>
        <CButton
        v-if="!this.$route.params.id"
         color="primary"  @click="createProduct()"
        >
          Submit
        </CButton>
        <CButton v-else color="primary" class="btn-click" @click="updateProduct(form.id)" >
          Update
        </CButton>

      </CCardFooter>
    </CCard>
  </div>
  </div>
</template>
<script>
import axios from "axios";
import swal from "sweetalert2";
import { URL_API } from "~/constant/constant";
export default {
  name: "CreateEditProduct",
  data() {
    return {
      form: {
        id: "",
        name: "",
        des: "",
        qty: "",
        discount: "",
        price: "",
        category_id: "",
        thumb: "",
      },
      errors: [],
      dataCategory: [],
    };
  },
  mounted() {
    if (this.$route.params.id != null) {
      this.getProductByID(this.$route.params.id);
    }
    this.getData();
  },
  props: {
    title: "",
  },
  methods: {
    /**
     * create product
     */
    createProduct() {
      console.log("OK");
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios.post(URL_API + "product", this.form).then((res) => {
          console.log("OK");
          this.$router.push("/product");
          swal.fire({
            position: "center",
            icon: "success",
            title: "Successfully Added",
            showConfirmButton: false,
            timer: 1500,
          });
        });
      }
    },

    /**
     * get product by id
     */
    getProductByID(id) {
      axios
        .get(URL_API + "product/" + id)
        .then((res) => (this.form = res.data.data));
    },

    /**
     * update product
     */
    updateProduct(id) {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios.put(URL_API + "product/" + id, this.form).then((res) => {
          this.$router.push("/product");
          swal.fire({
            position: "center",
            icon: "success",
            title: "Successfully Update",
            showConfirmButton: false,
            timer: 1500,
          });
        });
      }
    },

    /**
     * check validate
     */
    validate() {
      this.errors = [];
      if (this.form.subject == "") {
        this.errors.push("Name không được trống");
      }
      if (this.form.content == "") {
        this.errors.push("Content không được trống");
      }
    },

    /**
     * get list category
     */
    getData() {
      axios.get(URL_API + "category").then((res) => {
        this.dataCategory = res.data.data;
        console.log(this.dataCategory);
      });
    },
  },
};
</script>
<style lang="">
</style>