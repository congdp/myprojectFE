<template lang="">
  <div>
    <CCard>
      <CCardHeader >
        <h3>{{title}}</h3>
      </CCardHeader>
      <CCardBody>
        <CRow>
          <CCol sm="12">
            <CInput
              label="Name"
              placeholder="Enter project name"
              v-model="form.name"
            />
          </CCol>
        </CRow>
        <CRow>
          <CCol sm="12">
            <CInput label="Description" placeholder="Description"  v-model="form.des">
            </CInput>
          </CCol>
        </CRow>
        <CRow>
          <CCol sm="12">
            <CInput
              label="Start Date"
              type="date"
              v-model="form.start_date"
            >
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
</template>
<script>
import axios from "axios";
import swal from "sweetalert2";
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
        price:"",
        category_id:"",
        thumb:"",
      },
      errors: [],
    };
  },
  props:{
    title:"",
  },
  mounted() {
    if (this.$route.params.id != null) {
      this.getProductByID(this.$route.params.id);
    }
  },
  methods: {
    /**
     * create blog
     */
    createProduct() {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios
          .post("http://localhost:8000/api/product", this.form)
          .then((res) => {
            this.$router.push("/project");
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
        .get("http://localhost:8000/api/product/" + id)
        .then((res) => (this.form = res.data));
    },

    /**
     * update product
     */
    updateProduct(id) {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios
          .put("http://localhost:8000/api/product/" + id, this.form)
          .then((res) => {
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
      if (this.form.name == "") {
        this.errors.push("Name không được trống");
      }
      if (this.form.des == "") {
        this.errors.push("Description không được trống");
      }
    },
  },
};
</script>
<style lang="">
</style>
