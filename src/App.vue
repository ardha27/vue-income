<template>
  <MyHeader :totalIncome="state.totalIncome" />
  <MyForm @add-income="AddIncome"/>
  <IncomeList :state="state" />
</template>

<script>
import {reactive, computed} from 'vue'
import MyHeader from './components/MyHeader'
import MyForm from './components/MyForm'
import IncomeList from './components/IncomeList'

export default {
  components: {
    MyHeader,
    MyForm,
    IncomeList
},
  setup() {
    const state = reactive({
      income: [],
      totalIncome: computed(() =>{
        let temp = 0

        if (state.income.length > 0) {
          for (let i = 0; i < state.income.length; i++) {
            temp += state.income[i].value
          }
        }
        return temp;
      }),
      sortedIncome: computed(() => {
        let temp = []

        temp = state.income.sort(function (a, b) {
          return b.date - a.date
        })
        return temp
      })
    })

    function AddIncome(data) {
      let d = data.date.split("-")
      let newD = new Date(d[0], d[1], d[2])

      state.income = [...state.income, {
        id: Date.now(),
        desc: data.desc,
        value: parseInt(data.value),
        date: newD.getTime()
      }]
      console.log(state.income)
    }

    return {
      state,
      AddIncome,
      IncomeList
    }
  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

body {
  background: #eee;
}
</style>
