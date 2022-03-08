<template>
  <div id="app">
    <form class="form" @submit.prevent="sendForm">
      <div>
        <label>
          City:
          <select class="select_city" name="city" v-model="city">
            <option disabled value="">Choose city</option>
            <option v-for="city in cities" :key="city">{{ city }}</option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Department:
          <select
            class="select_department"
            name="department"
            v-model="department"
          >
            <option disabled value="">Choose department</option>
            <option v-for="department in departments" :key="department">
              {{ department }}
            </option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Employee:
          <select class="select_employee" name="employee" v-model="employee">
            <option disabled value="">Choose employee</option>
            <option v-for="a in employees" :key="a.employee">
              {{ a.employee }}
            </option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Brigade:
          <select
            class="select_brigade"
            name="brigade"
            v-model="brigade"
            @change="changeShift"
          >
            <option>Day</option>
            <option>Night</option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Shift:
          <select
            class="select_shift"
            name="shift"
            v-model="shift"
            @change="changeBrigade"
          >
            <option>08:00-20:00</option>
            <option>20:00-08:00</option>
          </select>
        </label>
      </div>
      <div class="buttons">
        <button type="submit" class="button send">Send</button>
        <button type="button" class="button reset" @click="reset">Reset</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [
        {
          employee: "Employee 1",
          city: "Paris",
          department: "Department 1",
        },
        {
          employee: "Employee 2",
          city: "London",
          department: "Department 2",
        },
        {
          employee: "Employee 3",
          city: "New York",
          department: "Department 3",
        },
        {
          employee: "Employee 4",
          city: "London",
          department: "Department 4",
        },
        {
          employee: "Employee 5",
          city: "Paris",
          department: "Department 5",
        },
        {
          employee: "Employee 6",
          city: "New York",
          department: "Department 6",
        },
        {
          employee: "Employee 7",
          city: "London",
          department: "Department 4",
        },
        {
          employee: "Employee 8",
          city: "New York",
          department: "Department 6",
        },
        {
          employee: "Employee 9",
          city: "Paris",
          department: "Department 5",
        },
        {
          employee: "Employee 10",
          city: "London",
          department: "Department 2",
        },
      ],
      city: "",
      department: "",
      employee: "",
      brigade: "Day",
      shift: "08:00-20:00",
    };
  },
  methods: {
    changeShift() {
      if (this.brigade === "Day") {
        this.shift = "08:00-20:00";
      } else {
        this.shift = "20:00-08:00";
      }
    },
    changeBrigade() {
      if (this.shift === "08:00-20:00") {
        this.brigade = "Day";
      } else {
        this.brigade = "Night";
      }
    },
    sendForm(event) {
      const data = new FormData(event.target);
      const dataArr = Array.from(data).filter((item) => !!item[1]);
      const dataObj = JSON.stringify(Object.fromEntries(dataArr));
      document.cookie = `${dataObj};secure;samesite=strict;max-age=3600`;
      console.log(dataObj);
      console.log(document.cookie);
    },
    reset() {
      this.city = "";
      this.department = "";
      this.employee = "";
      this.brigade = "Day";
      this.shift = "08:00-20:00";
    },
  },
  computed: {
    cities() {
      let cities = [];
      this.data.forEach((a) => {
        if (
          !cities.includes(a.city) &&
          (this.department ? a.department.includes(this.department) : true) &&
          (this.employee ? a.employee.includes(this.employee) : true)
        ) {
          cities.push(a.city);
        }
      });
      cities = Array.from(new Set(cities));

      return cities;
    },
    departments() {
      let departments = [];
      this.data.forEach((a) => {
        if (
          !departments.includes(a.department) &&
          (this.city ? a.city.includes(this.city) : true) &&
          (this.employee ? a.employee.includes(this.employee) : true)
        ) {
          departments.push(a.department);
        }
      });
      departments = new Set(departments);

      return Array.from(departments);
    },
    employees() {
      let employees = [];
      this.data.forEach((a) => {
        if (
          (this.department ? a.department.includes(this.department) : true) &&
          (this.city ? a.city.includes(this.city) : true)
        ) {
          employees.push(a);
        }
      });

      return employees;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  width: 800px;
  margin: 0 auto;
  margin-top: 60px;
}
.form {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  padding: 40px;
}

.form > div {
  margin-bottom: 40px;
}
select {
  padding: 4px;
}

.buttons {
  display: flex;
}
.button {
  width: 150px;
  padding: 10px;
  cursor: pointer;
  border-radius: 4px;
}
.reset {
  background: #fff;
  border: none;
  outline: 1px solid rgb(195, 193, 193);
  color: rgb(148, 145, 145);
}
.reset:hover {
  background: rgb(195, 193, 193);
  color: #fff;
  outline: none;
}
.send {
  margin-right: 40px;
  background: #22af39;
  border: none;
  color: #fff;
}
.send:hover {
  background: #249c38;
}
</style>
