<template>
  <CreateEditCategory :title="title"/>
</template>
<script>
import CreateEditCategory from '@/components/category/CreateEditCategory'
export default {
  components:{
    CreateEditCategory
  },
  data(){
    return{
      title: "Add Category"
    }
  }
}
</script>
<style>

</style>
