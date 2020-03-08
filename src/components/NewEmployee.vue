<template>
    <div class="new-employee">
        <h2>Add a new employee</h2>
        <form>
          <label for="name">Name</label>
          <input type="text" id="name" placeholder="name..." v-model="newEmployeeName">

          <label for="surname">Surname</label>
          <input type="text" id="surname" placeholder="surname..." v-model="newEmployeeSurname">

          <label for="departament">Section</label>
          <select name="section" v-model="newEmployeeDepartament">
            <option v-for="departament in departaments" :value="departament.value" :key="departament.value">
              {{departament.value}}
            </option>
          </select>

          <label for="payment">Payment</label>
          <input type="number" id="payment" placeholder="amount..." min="0" v-model="newEmployeePayment">

          <button @click="addEmployee">Dodaj</button>
        </form>
      </div>
</template>

<script>
export default {
    name: 'NewEmployee',
    props: {
        employees: Array,
        departaments: Array,
    },
    data(){
        return{
            newEmployeeName: '',
            newEmployeePayment: '',
            newEmployeeSurname: '',
            newEmployeeDepartament: '',
        }
    },
    methods: {
        capitalize(string){
            return string.charAt(0).toUpperCase() + string.slice(1);
        },
        clearInput(){
            this.newEmployeeName = '';
            this.newEmployeeSurname = '';
            this.newEmployeeDepartament = '';
            this.newEmployeePayment = '';
        },
        addEmployee(e){
            e.preventDefault();
            if(this.newEmployeeName !== "" && this.newEmployeeFromPayment !== "" && this.newEmployeeToPayment !== "" && this.newEmployeeDepartament !== "" && this.newEmployeeSurname !== ""){
                const employee = {
                    name: this.capitalize(this.newEmployeeName),
                    surname: this.capitalize(this.newEmployeeSurname),
                    section: this.newEmployeeDepartament,
                    paymentAmount: this.newEmployeePayment,
                    currency: 'PLN'
                }
                this.clearInput();
                this.employees.push(employee)
            } else alert('All fields required')
        },
    },
}
</script>

<style>
.new-employee form{
    display: flex;
    flex-direction: column;
}
</style>