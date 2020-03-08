<template>
    <div class="filter">
        <h2>Filter employees</h2>
        <div class="search-person">
            <p>Search for person</p>
            <label style="margin-top:0" for="filterName">Name</label>
            <input type="text" id="filterName" placeholder="name..." v-model="filterName">
            <label for="filterSurname">Surname</label>
            <input type="text" id="filterSurname" placeholder="surname..." v-model="fiterSurname">
            <button id="personBtn" @click="filtering">Search</button>
        </div>
          
        <div class="search-departament">
            <p>Search by section</p>
            <select name="filterDepartament" v-model="filterDepartament">
                <option v-for="departament in departaments" :value="departament.value" :key="departament.value">
                    {{departament.value}}
                </option>
            </select>
            <button id="departamentBtn" @click="filtering">Search</button>
        </div>
          
        <div class="search-amount">
            <p>Search by payment</p>
            <div class="set-amount">
                <p>from</p><input type="number" id="amountFrom" placeholder="0" min="0" max="25000" v-model="filterFromAmount">
                <p>to</p><input type="number" id="amountTo" placeholder="5000" min="0" max="25000" v-model="filterToAmount">
            </div>
            <button id="paymentBtn" @click="filtering">Search</button>
        </div>

        <div class="results" v-if="filterResults.length > 0">
            <div class="found">
                <h3>Found persons - {{filterResults.length}}</h3>
                <div class="closeBtn" @click="closeResults">X</div>
            </div>
            <p v-for="(person, index) in filterResults" :key="index">
                {{person.name}} {{person.surname}} - {{person.section}} - {{person.paymentAmount}} PLN
            </p>
        </div>
    </div>
</template>

<script>


export default {
    name: 'FilterSection',
    props: {
        employees: Array,
        departaments: Array,
    },
    data(){
        return{
            filterName: '',
            fiterSurname: '',
            filterDepartament: '',
            filterFromAmount: '',
            filterToAmount: '',
            filterResults: [],
        }
    },
    methods: {
        filtering(e){
            e.preventDefault();
            this.filterResults = [];
            const id = e.target.id;
            if(id === "personBtn"){
                const filtered = this.employees.filter(employee => {
                    const name = employee.name.toLowerCase();
                    const surname = employee.surname.toLowerCase();
                    return (name.indexOf(this.filterName.toLowerCase()) > -1) && (surname.indexOf(this.fiterSurname.toLowerCase()) > -1)
                })
                if(filtered.length > 0) this.filterResults = filtered
                else {
                    const section = '.search-person';
                    this.showNotFoundMessage(section);
                }
            }else if(id === "departamentBtn"){
                const filtered = this.employees.filter(employee => {
                    const departament = employee.section.toLowerCase();
                    return departament.indexOf(this.filterDepartament.toLowerCase()) > -1
                })
                if(this.filterDepartament === '') return null
                if(filtered.length > 0) this.filterResults = filtered
                else {
                    const section = '.search-departament';
                    this.showNotFoundMessage(section);
                }
            }else if(id === "paymentBtn"){
                const filtered = this.employees.filter(employee => {
                    const payment = parseFloat(employee.paymentAmount);
                    return (payment >= this.filterFromAmount) && (payment <= this.filterToAmount)
                })
                if(filtered.length > 0) this.filterResults = filtered
                else {
                    const section = '.search-amount';
                    this.showNotFoundMessage(section);
                }
            }else return null
        },
        closeResults(){
            this.filterResults = [];
        },
        showNotFoundMessage(section){
            const p = document.createElement('p');
            p.style="font-size: 1.8rem; text-align: center; margin-top: .5rem; color: #640101;";
            p.innerText = 'Not found';
            const sec = document.querySelector(section);
            sec.appendChild(p);
                setTimeout(()=>{
                    sec.removeChild(p);
            }, 2500)
        }
    },
}
</script>

<style scoped>
p{font-size: 2rem; text-align: center;}
h3{font-size: 2.4rem;}
.search-person,
.search-departament,
.search-amount{
    display: flex;
    flex-direction: column;
    border-bottom: 1px solid #000;
    padding: 1.5rem 0 2rem 0;
}
.search-amount{border: none;}
.search-amount p,
.search-departament p{
    margin-bottom: .8rem;
}
.set-amount{display: flex;}
.set-amount p{padding: 0 .5rem; margin: 0;}
.results{
    position: fixed;
    top: 50%; left: 50%; transform: translate(-50%,-50%);
    background-color: #222;
    min-width: 30%;
    color: #fff;
    padding: 10px 20px;
}
.results p{padding: .5rem 0; font-size: 2.2rem;}
.found{
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2rem;
}
.closeBtn{
    background-color: #fff;
    color: #000;
    width: 3rem;
    height: 3rem;
    text-align: center;
    font-size: 2rem;
    line-height: 2.5rem;
    font-family: arial, 'sans-serif';
    border-radius: 50%;
    padding: 3px;
    cursor: pointer;
}
</style>