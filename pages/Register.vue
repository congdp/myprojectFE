<template>
  <CContainer class="min-vh-100 d-flex align-items-center">
    <CRow class="w-100 justify-content-center">
      <CCol md="6" sm="8">
        <CCard class="mx-4 mb-0">
          <CCardBody class="p-4">
            <CForm>
              <h1>Register</h1>
              <p class="text-muted">Create your account</p>
              <CInput
                placeholder="Username"
                autocomplete="username"
                v-model="form.name"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilUser" />
                </template>
              </CInput>
              <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.name }}
              </p>
              <CInput
                placeholder="Email"
                autocomplete="email"
                v-model="form.email"
                prepend="@"
              />
               <p v-if="Object.keys(errors).length > 0" class="text text-danger">
            {{ errors.email }}
          </p>
              <CInput
                placeholder="Password"
                type="password"
                autocomplete="new-password"
                v-model="form.password"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilLockLocked" />
                </template>
              </CInput>
               <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.password }}
              </p>
              <CInput
                placeholder="Repeat password"
                type="password"
                class="mb-4"
                v-model="form.password_confirmation"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilLockLocked" />
                </template>
              </CInput>
               <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.password_confirmation }}
              </p>
              <CButton color="success" block @click="register">
                Create Account
              </CButton>
            </CForm>
          </CCardBody>
          <!-- <CCardFooter class="p-4">
            <CRow>
              <CCol col="6">
                <CButton block color="facebook"> Facebook </CButton>
              </CCol>
              <CCol col="6">
                <CButton block color="twitter"> Twitter </CButton>
              </CCol>
            </CRow>
          </CCardFooter> -->
        </CCard>
      </CCol>
    </CRow>
  </CContainer>
</template>

<script>
import { freeSet } from "@coreui/icons";
import axios from "axios";
export default {
  auth: false,
  name: "Register",
  freeSet,
  data() {
    return {
      form: {
        name: "",
        email: "",
        password: "",
        password_confirmation: "",
      },
      errors: {},
    };
  },
  methods: {
    /**
     * check validate
     */
    validate() {
      this.errors = {};
      if (this.form.name == "") {
        this.errors.name = "Name không được trống";
      }
      if (this.form.password == "") {
        this.errors.password = "Password không được trống";
      }
      if (this.form.password !== this.form.password_confirmation) {
        this.errors.password_confirmation = "Password không giống";
      }
      if (this.form.email == "") {
        this.errors.email = "Email không được trống";
      }
    },
    register() {
      this.validate();
      if (Object.keys(this.errors).length > 0) {
        console.log(this.errors);
        return this.errors;
      } else {
        axios
          .post("http://127.0.0.1:8000/api/auth/signup", this.form)
          .then((res) => {
            this.$router.push("/login");
          });
      }
    },
  },
};
</script>
