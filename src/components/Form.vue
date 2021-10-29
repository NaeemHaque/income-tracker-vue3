<template>
<form @submit.prevent="formHandler">
  <input type="text" placeholder="Description" v-model="formData.desc">
  <input type="number" placeholder="Value" v-model="formData.value">
  <input type="date" pattern="Date" v-model="formData.date">
  <input type="submit" value="SUBMIT">
<!--  <p v-if="invallid">Please Fillup All Data</p>-->
</form>
</template>

<script>
import {reactive} from "vue";

export default {
  props : {
    state: Object
  },

  setup(props, {emit}) {
    // eslint-disable-next-line no-unused-vars
    // const invallid = ref(false);

    const formData = reactive({
      desc: null,
      value: null,
      date: null,
      isNeg: false
    })

    function formHandler() {
      // if (formData.desc == '' || formData.value == '' || formData.date == '') {
      //   invallid.value = true;
      //   return
      // }
      if (formData.value < 0) {
        formData.isNeg = true
      }
      emit("add-income", {
        desc: formData.desc,
        value: formData.value,
        date: formData.date,
        isNeg: formData.isNeg
      })
      formData.desc = null
      formData.value = null
      formData.date = null
      formData.isNeg = false

    }

    return {
      formData,
      formHandler,
      // invallid
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  justify-content: center;
  margin-top: 30px;
}
form input {
  color: #888;
  border: none;
  outline: none;
  font-size: 20px;
}
form input::placeholder {
  color: #AAA;
}
form input:not([type="submit"]) {
  display: block;
  background: #FFF;
  border: none;
  outline: none;
  padding: 5px 15px;
}
form input[type="submit"] {
  display: block;
  background: none;
  border: none;
  outline: none;
  color: #000;
  font-weight: 500;
  text-shadow: 0px 1px 3px rgba(0, 0, 0, 0.2);
  padding: 5px 15px;
  background-color: #FFCE00;
  cursor: pointer;
}
form input:first-of-type {
  border-radius: 8px 0px 0px 8px;
}
form input:last-of-type {
  border-radius: 0px 8px 8px 0px;
}
</style>