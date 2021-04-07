
<template lang="">
  <div>
    <CreateEditProduct :title="title"/>
  </div>
</template>
<script>
import CreateEditProduct from "@/components/product/CreateEditProduct"
export default {
  components:{
    CreateEditProduct
  },
   data(){
    return{
      title: "Edit Product"
    }
  }
}
</script>
<style lang="">

</style>
