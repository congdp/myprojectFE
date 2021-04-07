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
        <!-- <CRow>
          <CCol sm="12">
            <CInput
              label="Start Date"
              type="date"
              v-model="form.start_date"
            >
            </CInput>
          </CCol>
        </CRow> -->
        <CRow>
          <ul v-if="errors.length > 0" class="alert alert-danger">
          <li v-for="error in errors" :key="error">{{ error }}</li>
         </ul>
        </CRow>
      </CCardBody>
      <CCardFooter>
        <CButton
        v-if="!this.$route.params.id"
         color="primary"  @click="createCategory()"
        >
          Submit
        </CButton>
        <CButton v-else color="primary" class="btn-click" @click="updateCategory(form.id)" >
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
  name: "CreateEditCategory",
  data() {
    return {
      form: {
        id: "",
        name: "",
        des: "",
      },
      errors: [],
    };
  },
  props:{
    title:"",
  },
  mounted() {
    if (this.$route.params.id != null) {
      this.getCategoryByID(this.$route.params.id);
    }
  },
  methods: {
    /**
     * create blog
     */
    createCategory() {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios
          .post("http://localhost:8000/api/category", this.form)
          .then((res) => {
            this.$router.push("/category");
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
     * get blog by id
     */
    getCategoryByID(id) {
      axios
        .get("http://localhost:8000/api/category/" + id)
        .then((res) => (this.form = res.data));
    },
    /**
     * update blog
     */
    updateCategory(id) {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        axios
          .put("http://localhost:8000/api/category/" + id, this.form)
          .then((res) => {
            this.$router.push("/category");
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
