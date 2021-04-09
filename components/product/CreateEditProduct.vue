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
          <!-- <CCol sm="4"> -->
             <!-- <CInputFile label="Thumb" @change="getFile" /> -->
          <!-- </CCol> -->
         <CCol sm="4">
            <label>Thumb</label>
            <img class= "thumb-preview mb-2" v-bind:src="imagePreview" v-show="showPreview"/>
            <input type="file" id="file" ref="file"   accept="image/*" v-on:change="handleFileUpload()"/>
          </CCol>
        </CRow>
         <CRow>
           <CCol sm="4" class= "mt-3">
             <ul v-if="errors.length > 0" class="alert alert-danger">
             <li v-for="error in errors" :key="error">{{ error }}</li>
             </ul>
          </CCol>
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
      showPreview: false,
      imagePreview: "",
      // file: "",
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
    async createProduct() {
      // console.log("OK");
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
        // console.log(this.form.thumb.target.files[0]);
        const data = new FormData();
        data.append("category_id", this.form.category_id);
        data.append("name", this.form.name);
        data.append("des", this.form.des);
        data.append("price", this.form.price);
        data.append("discount", this.form.discount);
        data.append("qty", this.form.qty);
        data.append("thumb", this.form.thumb);
        console.log(data.get("thumb"));
        const res = await axios.post(URL_API + "product", data, {
          "Content-type": "multipart/form-data",
        });

        swal.fire({
          position: "center",
          icon: "success",
          title: "Successfully Added",
          showConfirmButton: false,
          timer: 1500,
        });
        if (res) {
          this.$router.push("/product");
        }
      }
    },
    
    /**
     * get thumb upload
     */
    handleFileUpload() {
      this.form.thumb = this.$refs.file.files[0];

      let reader = new FileReader();

      reader.addEventListener(
        "load",
        function () {
          this.showPreview = true;
          this.imagePreview = reader.result;
        }.bind(this),
        false
      );

      if (this.form.thumb) {
        if (/\.(jpe?g|png|gif)$/i.test(this.form.thumb.name)) {
          reader.readAsDataURL(this.form.thumb);
        }
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
    async updateProduct(id) {
      this.validate();
      if (this.errors.length > 0) {
        return this.errors;
      } else {
         const data = new FormData();
        data.append("category_id", this.form.category_id);
        data.append("name", this.form.name);
        data.append("des", this.form.des);
        data.append("price", this.form.price);
        data.append("discount", this.form.discount);
        data.append("qty", this.form.qty);
        data.append("thumb", this.form.thumb);
        console.log(data.get("thumb"));
         const res = await axios.put(URL_API + "product" + id, data, {
          "Content-type": "multipart/form-data",
        });

        swal.fire({
          position: "center",
          icon: "success",
          title: "Successfully Updated",
          showConfirmButton: false,
          timer: 1500,
        });
        if (res) {
          this.$router.push("/product");
        }
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
      if (this.form.category_id == "") {
        this.errors.push("Category không được trống");
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
<style >
.thumb-preview {
  display: block !important;
  width: 200px;
  height: 200px;
}
</style>