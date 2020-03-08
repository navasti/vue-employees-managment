<template>
    <div class="employees">
      <table>
          <thead>
            <tr>
              <th>Name</th>
              <th>Surname</th>
              <th>Section</th>
              <th>Payment</th>
              <th>Currency</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(employee, index) in employees" :key="index">
              <th>{{employee.name}}</th>
              <th>{{employee.surname}}</th>
              <th>{{employee.section}}</th>
              <th>{{employee.paymentAmount}}</th>
              <th>{{employee.currency}}</th>
            </tr>
          </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'Employees',
    props: {
        employees: Array,
    },
}
</script>

<style>
.employees{
  display: flex;
  justify-content: center;
  margin: 1rem 0 3rem 0;
}
table th{
  padding: .5rem 1.5rem;
  font-size: 2rem;
  border: 1px solid rgba(0,0,0,.3);
}
table thead tr th{
  font-size: 2.4rem;
  letter-spacing: 2px;
}
</style>