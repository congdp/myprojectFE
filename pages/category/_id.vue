
<template lang="">
  <div>
    <CreateEditCategory :title="title"/>
  </div>
</template>
<script>
import CreateEditCategory from "@/components/category/CreateEditCategory"
export default {
  components:{
    CreateEditCategory
  },
   data(){
    return{
      title: "Edit Category"
    }
  }
}
</script>
<style lang="">

</style>
