<template>
<div class="box">
 <CRow>
    <CCol sm="4" class="search position-relative">
      <CInput  placeholder="Enter your key search" v-model="keyword" />
      <CButton  class="btn-sm btn-search position-absolute" @click="search">
        <CIcon :content="$options.freeSet.cilMagnifyingGlass" />
      </CButton>
    </CCol>
  </CRow>
</div>

</template>
<script>
import { freeSet } from "@coreui/icons";
export default {
  freeSet,
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    search(){
      this.$emit('sendKeyword', this.keyword);
    }
  }
};
</script>
<style scoped>
.search {
top: 67px;
    left: 65%;
    z-index: 1000;
}
.btn-search {
top:1px;
right: 17px;
}
</style>
