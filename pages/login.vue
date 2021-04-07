<template>
  <div class="c-app flex-row align-items-center">
    <CContainer>
      <CRow class="justify-content-center">
        <CCol md="8">
          <CCardGroup>
            <CCard class="p-4">
              <CCardBody>
                <CForm>
                  <h1>Login</h1>
                  <p class="text-muted">Sign In to your account</p>
                   <p
                    v-if="Object.keys(err).length > 0"
                    class="text text-danger"
                  >
                    {{ err.fail }}
                  </p>
                  <CInput
                    placeholder="Username"
                    autocomplete="username email"
                    v-model="userForm.email"
                  >
                    <template #prepend-content
                      ><CIcon :content="$options.freeSet.cilUser"
                    /></template>
                  </CInput>
                  <p
                    v-if="Object.keys(err).length > 0"
                    class="text text-danger"
                  >
                    {{ err.email }}
                  </p>
                  <CInput
                    placeholder="Password"
                    type="password"
                    autocomplete="curent-password"
                    v-model="userForm.password"
                  >
                    <template #prepend-content
                      ><CIcon :content="$options.freeSet.cilLockLocked"
                    /></template>
                  </CInput>
                  <p
                    v-if="Object.keys(err).length > 0"
                    class="text text-danger"
                  >
                    {{ err.password }}
                  </p>
                  <CRow>
                    <CCol col="6" class="text-left">
                      <CButton color="primary" class="px-4" @click="login"
                        >Login</CButton
                      >
                    </CCol>
                    <CCol col="6" class="text-right">
                      <CButton color="link" class="px-0"
                        >Forgot password?</CButton
                      >
                      <CButton color="link" class="d-lg-none"
                        >Register now!</CButton
                      >
                    </CCol>
                  </CRow>
                </CForm>
              </CCardBody>
            </CCard>
            <CCard
              color="primary"
              text-color="white"
              class="text-center py-5 d-md-down-none"
              body-wrapper
            >
              <CCardBody>
                <h2>Sign up</h2>
                <p>
                  Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed
                  do eiusmod tempor incididunt ut labore et dolore magna aliqua.
                </p>
                <CButton color="light" variant="outline" size="lg" @click="register">
                  Register Now!
                </CButton>
              </CCardBody>
            </CCard>
          </CCardGroup>
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>
<script>
import { freeSet } from "@coreui/icons";
export default {
  name: "Login",
  freeSet,
  data() {
    return {
      userForm: {
        email: "",
        password: "",
      },
      err: {},
    };
  },
  methods: {
    async login() {
      this.validate();
      if (Object.keys(this.err).length > 0) {
        return this.err;
      } else {
        try {
          await this.$auth
            .login({
              data: this.userForm,
            })
            .then((res) => {
              this.$router.push("/");
            });
        } catch (err) {
          this.err.fail = "Username or Password not valid";
          console.log(this.err.fail);
        }
      }
    },
    validate() {
      this.err = {};
      if (this.userForm.email === "") {
        this.err.email = "email không được để trống";
      } else if (this.userForm.password === "") {
        this.err.password = "mật khẩu không được để trống";
      }
    },
    register(){
      this.$router.push('/Register')
    }
  },
};
</script>
