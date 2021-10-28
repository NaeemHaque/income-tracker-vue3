<template>
  <div class="income-list-item">
    <div class="desc">{{ income.desc }}</div>
    <div class="value">${{ income.value }}</div>
    <div class="date">{{ formattedDate }}</div>
    <div class="removeItem" @click="removeItem">x</div>

  </div>
</template>

<script>
export default {
  props: {
   income: Object
  },

  setup (props, { emit }) {

    let date = new Date(props.income.date);
    let day = date.getDate();
    let month = date.getMonth();
    let year = date.getFullYear();
    let formattedDate = day + "/" + month + "/" + year;

    function removeItem () {
      emit("remove-item", props.income.id);
    }

    return {
      formattedDate,
      removeItem
    }
  }
}
</script>

<style scoped>
.income-list-item {
  position: relative;
  display: flex;
  padding: 15px 15px 15px 10px;
  background-color: #FFF;
  border-radius: 8px;
  max-width: 600px;
  margin: 0 auto 30px;
  box-shadow: 2px 3px 7px lightgrey;
}
.removeItem {
  color: #EF2D2D;
  font-weight: 600;
  font-size: 20px;
  line-height: 1;
  text-align: center;
  margin-left: 40px;
  cursor: pointer;
}
.desc {
  color: #666;
  flex: 1 1 100%;
  font-size: 20px;
}
.value {
  color: #666;
  min-width: 100px;
  font-size: 20px;
}
.date {
  color: #666;
  text-align: right;
  font-size: 20px;
}
</style>