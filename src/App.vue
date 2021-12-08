<template>
  <div id="app">
    <form class="form" @submit.prevent="sendForm">
      <div>
        <label>
          Город:
          <select class="select_city" name="city" v-model="city">
            <option disabled value="">Выберите город</option>
            <option v-for="city in cities" :key="city">{{ city }}</option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Цех:
          <select
            class="select_department"
            name="department"
            v-model="department"
          >
            <option disabled value="">Выберите цех</option>
            <option v-for="department in departments" :key="department">
              {{ department }}
            </option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Сотрудник:
          <select class="select_employee" name="employee" v-model="employee">
            <option disabled value="">Выберите сотрудника</option>
            <option v-for="a in employees" :key="a.employee">
              {{ a.employee }}
            </option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Бригада:
          <select
            class="select_brigade"
            name="brigade"
            v-model="brigade"
            @change="changeShift"
          >
            <option>Дневная</option>
            <option>Ночная</option>
          </select>
        </label>
      </div>
      <div>
        <label>
          Смена:
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
        <button type="submit" class="button send">Отправить</button>
        <button type="button" class="button reset" @click="reset">
          Сбросить
        </button>
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
          employee: "Сотрудник 1",
          city: "Москва",
          department: "Цех 1",
        },
        {
          employee: "Сотрудник 2",
          city: "Сочи",
          department: "Цех 2",
        },
        {
          employee: "Сотрудник 3",
          city: "Новосибирск",
          department: "Цех 3",
        },
        {
          employee: "Сотрудник 4",
          city: "Сочи",
          department: "Цех 4",
        },
        {
          employee: "Сотрудник 5",
          city: "Москва",
          department: "Цех 5",
        },
        {
          employee: "Сотрудник 6",
          city: "Новосибирск",
          department: "Цех 6",
        },
        {
          employee: "Сотрудник 7",
          city: "Сочи",
          department: "Цех 4",
        },
        {
          employee: "Сотрудник 8",
          city: "Новосибирск",
          department: "Цех 6",
        },
        {
          employee: "Сотрудник 9",
          city: "Москва",
          department: "Цех 5",
        },
        {
          employee: "Сотрудник 10",
          city: "Сочи",
          department: "Цех 2",
        },
      ],
      city: "",
      department: "",
      employee: "",
      brigade: "Дневная",
      shift: "08:00-20:00",
    };
  },
  methods: {
    changeShift() {
      if (this.brigade === "Дневная") {
        this.shift = "08:00-20:00";
      } else {
        this.shift = "20:00-08:00";
      }
    },
    changeBrigade() {
      if (this.shift === "08:00-20:00") {
        this.brigade = "Дневная";
      } else {
        this.brigade = "Ночная";
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
      this.brigade = "Дневная";
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
