<template>

   <Header :totalIncome = "state.totalIncome" />
   <Form @add-income="AddIncome"/>
   <IncomeList :state="state" @remove-item="removeItem"/>

</template>

<script>
import {computed, reactive} from "vue";

import Header from './components/Header'
import Form from './components/Form'
import IncomeList from './components/IncomeList'
export default {
  setup(){
    const state = reactive({
      income: [],
      totalIncome: computed(()=>{
        let temp = 0;

        if (state.income.length >= 0 ){
          for (let i=0; i< state.income.length; i++) {
            temp += state.income[i].value;
          }
        }

        return temp
      }),

      sortedIncome: computed( () => {
        let temp =[]

        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        temp = state.income.sort( function (a,b) {
          return a.date - b.date
        } )

        return temp
      } )
    })


    function AddIncome(data) {
      let d = data.date.split("-")
      let newD = new Date(d[0], d[1], d[2])
      state.income = [...state.income, {
        id: Date.now(),
        desc: data.desc,
        value: parseInt(data.value),
        date: newD.getTime(),
        isNeg: data.isNeg
      }]
    }
    function removeItem(id) {
      state.income = state.income.filter(v => v.id != id);
    }


    return {
      state,
      AddIncome,
      IncomeList,
      removeItem
    }
  },
  components: {
    IncomeList,
    Form,
    Header
  },

}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}
body {
  background: #2c2c30;
}

</style>
