<template>
  <body>
     <Header :totalIncome = "state.totalIncome" />
     <Form @add-income="AddIncome"/>
     <IncomeList :state="state" @remove-item="removeItem"/>
     <button class="download" @click="downloadPdf">Download</button>
   </body>

</template>

<script>
import {computed, reactive} from "vue";
import { jsPDF } from "jspdf";

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

    function downloadPdf(){
      let doc = new jsPDF({
        unit: 'mm',
        format: 'a4',
        putOnlyUsedFonts:true,
        floatPrecision: 16 // or "smart", default is 16
      });
      doc.html(document.body, {
        callback: function (doc) {
          doc.save("income-tracker.pdf");
        },
        x: 10,
        y: 10,
        width: 190,
        windowWidth: 1000
      });
    }


    return {
      state,
      AddIncome,
      IncomeList,
      removeItem,
      downloadPdf
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
.download {
  font-family: 'Fira Code', 'Fira Sans', sans-serif;
  background-color: #FFCE00;
  color: #000;
  font-size: 20px;
  font-weight: 900;
  padding: 5px 10px;
  width: 130px;
  text-align: center;
  border-radius: 8px;
  text-shadow: 0px 3px 3px rgba(0, 0, 0, 0.25);
  box-shadow: 2px 3px 6px lightgrey;
  display: flex;
  margin: 0 auto !important;
  cursor: pointer;

}
</style>
