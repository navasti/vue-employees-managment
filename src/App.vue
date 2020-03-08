<template>
  <div class="wrapper">
    <Employees :employees="employees"/>
    <main>
      <NewEmployee :employees="employees" :departaments="departaments"/>
      <TotalPayments 
      :ITPayments="ITPayments" 
      :administrationPayments="administrationPayments"
      :merchantPayments="merchantPayments"
      />
      <FilterSection :employees="employees" :departaments="departaments"/>
    </main>
  </div>

</template>

<script>
import NewEmployee from './components/NewEmployee.vue';
import FilterSection from './components/FilterSection.vue';
import TotalPayments from './components/TotalPayments.vue';
import Employees from './components/Employees.vue';

export default {
  name: 'App',
  components: {
    NewEmployee,
    FilterSection,
    TotalPayments,
    Employees
  },
  data(){
    return{
      employees: [],
      departaments: [],
      ITPayments: null,
      administrationPayments: null,
      merchantPayments: null,
    }
  },
  methods: {
    getDepartaments(){
      const departaments = [];
      this.employees.forEach(employee => {
        const filtered = departaments.find(departament => departament.value === employee.section);
        if(filtered) return null;
        departaments.push({ value: employee.section })
      })
      this.departaments = departaments
    },
    countPayments(){
      const it = [];
      let itTotal = 0;
      const administration = [];
      let administrationTotal = 0;
      const merchant = [];
      let merchantTotal = 0;
      this.employees.forEach(employee => {
        const payment = parseFloat(employee.paymentAmount);
        if(employee.section === "IT") it.push(payment);
        if(employee.section === "Administration") administration.push(payment);
        if(employee.section === "Merchant") merchant.push(payment);
      })
      if(it.length>0) it.forEach(payment => itTotal += payment)
      if(administration.length>0) administration.forEach(payment => administrationTotal += payment)
      if(merchant.length>0) merchant.forEach(payment => merchantTotal += payment)
      this.ITPayments = itTotal;
      this.administrationPayments = administrationTotal;
      this.merchantPayments = merchantTotal;
    }
  },
  created(){
    fetch(`/employees.json`)
      .then(res => res.json())
      .then(data => {
        data.forEach(employee => this.employees.push(employee))
        this.getDepartaments();
      })
      .catch(err => console.log(err))
  },
  updated(){
    this.countPayments()
  },
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Poppins:300&display=swap');
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
html{
  font-family: 'Poppins', sans-serif;
  font-size: 10px;
}
h2{
  font-size: 2.4rem;
}
label{
  font-size: 2rem;
  margin: .5rem 0;
}
input,
button,
option,
select{
  padding: .3rem .6rem;
  font-size: 1.8rem;
}
 button{
   margin-top: 1rem;
 }
 main{
  display: flex;
  justify-content: center;
  padding: 2rem;
}
</style>
