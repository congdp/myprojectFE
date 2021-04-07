<template>
  <CreateEditProduct :title="title"/>
</template>
<script>
import CreateEditProduct from '@/components/product/CreateEditProduct'
export default {
  components:{
    CreateEditProduct
  },
  data(){
    return{
      title: "Add Product"
    }
  }
}
</script>
<style>

</style>
