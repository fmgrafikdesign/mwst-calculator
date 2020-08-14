<template>
  <div class="form-group row justify-content-center" :class="name">
    <label :for="name" class="col-2 col-form-label-lg text-capitalize text-sm-right">{{ name }}:</label>
    <div class="col-10 col-sm-4 text-left">
      <input autocomplete="off" :id="name" :name="name" class="form-control-lg form-control" type="text" :value="model" @change="$emit('update',$event.target.value)">
    </div>
  </div>
</template>

<script>
export default {
  name: "InputComponent",
  props: ['name', 'model']
}
</script>

<style scoped>

</style>
