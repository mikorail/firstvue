<template>
  <div id="app" class="small-container">
    <h1>EmployeeTable</h1>
    <employee-form @add:employee="addEmployee" />

    <employee-table :employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee" />
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeForm,
      EmployeeTable,
    },
    methods: {
      addEmployee(employee) {
        const lastId =
          this.employees.length > 0 ?
          this.employees[this.employees.length - 1].id :
          0;
        const id = lastId + 1;
        const newEmployee = {
          ...employee,
          id
        };

        this.employees = [...this.employees, newEmployee];
      },
      deleteEmployee(id) {
        this.employees = this.employees.filter(
          employee => employee.id !== id
        )
      },
      editEmployee(id, updatedEmployee) {
        this.employee = this.employees.map(employee =>
          employee.id === id ? updatedEmployee : employee
        )
      }
    },
    data() {
      return {
        employees: [{
            id: 1,
            name: 'Richard Hendricks',
            email: 'richard@piedpiper.com',
          },
          {
            id: 2,
            name: 'Bertram Gilfoyle',
            email: 'gilfoyle@piedpiper.com',
          },
          {
            id: 3,
            name: 'Dinesh Chugtai',
            email: 'dinesh@piedpiper.com',
          },
        ],
      }
    },
    mounted() {
      this.getEmployees()
    },
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      } catch (error) {
        console.error(error)
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {
            'Content-type': 'application/json; charset=UTF-8'
          },
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(error)
      }
    }
  }
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>