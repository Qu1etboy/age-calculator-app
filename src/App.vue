<script setup lang="ts"></script>

<template>
  <div>
    <div class="box">
      <div class="form-container">
        <form class="form">
          <div class="input-box">
            <label :class="{ 'label-error': errorDay }">DAY</label>
            <input
              v-model="inputDay"
              placeholder="DD"
              :class="{ 'input-error': errorDay }"
            />
            <p v-show="errorDay" class="error">
              {{ errorDayMsg }}
            </p>
          </div>
          <div class="input-box">
            <label :class="{ 'label-error': errorMonth }">MONTH</label>
            <input
              v-model="inputMonth"
              placeholder="MM"
              :class="{ 'input-error': errorMonth }"
            />
            <p v-show="errorMonth" class="error">
              {{ errorMonthMsg }}
            </p>
          </div>
          <div class="input-box">
            <label :class="{ 'label-error': errorYear }">YEAR</label>
            <input
              v-model="inputYear"
              placeholder="YYYY"
              :class="{ 'input-error': errorYear }"
            />
            <p v-show="errorYear" class="error">
              {{ errorYearMsg }}
            </p>
          </div>
        </form>
        <button class="icon-arrow" v-on:click="calculateDate">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="46"
            height="44"
            viewBox="0 0 46 44"
          >
            <g fill="none" stroke="#FFF" stroke-width="2">
              <path
                d="M1 22.019C8.333 21.686 23 25.616 23 44M23 44V0M45 22.019C37.667 21.686 23 25.616 23 44"
              />
            </g>
          </svg>
        </button>
      </div>

      <div>
        <div class="output">
          <span class="number">{{ years }}</span> years
        </div>
        <div class="output">
          <span class="number">{{ months }}</span> months
        </div>
        <div class="output">
          <span class="number">{{ days }}</span> days
        </div>
      </div>
    </div>

    <footer class="attribution">
      Challenge by
      <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
        >Frontend Mentor</a
      >. Coded by <a href="https://qu1etboy.dev">Qu1etboy</a>.
    </footer>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      inputYear: "",
      inputMonth: "",
      inputDay: "",
      years: "--",
      months: "--",
      days: "--",
      errorYear: false,
      errorMonth: false,
      errorDay: false,
      errorYearMsg: "",
      errorMonthMsg: "",
      errorDayMsg: "",
    };
  },

  methods: {
    calculateDate() {
      let error = this.checkError(
        this.inputDay,
        this.inputMonth,
        this.inputYear
      );

      if (error) {
        return;
      }

      const today = new Date();

      this.errorDay = false;
      this.errorMonth = false;
      this.errorYear = false;

      const givenDate = new Date(
        [this.inputYear, this.inputMonth, this.inputDay].join("-")
      );

      // Calculate the difference in milliseconds between the two dates
      const difference = today.getTime() - givenDate.getTime();

      // Convert the difference from milliseconds to days
      const age = Math.floor(difference / (1000 * 60 * 60 * 24 * 365.25));

      this.years = age.toString().padStart(2, "0");
      this.months = givenDate.getMonth().toString().padStart(2, "0");
      this.days = givenDate.getDate().toString().padStart(2, "0");
    },

    checkError(day: string, month: string, year: string) {
      let error = false;

      const today = new Date();

      if (year === "") {
        this.errorYear = true;
        this.errorYearMsg = "This field is required";
        error = true;
      }

      if (month === "") {
        this.errorMonth = true;
        this.errorMonthMsg = "This field is required";
        error = true;
      }

      if (day === "") {
        this.errorDay = true;
        this.errorDayMsg = "This field is required";
        error = true;
      }

      if (parseInt(day) < 0 || parseInt(day) > 31) {
        this.errorDay = true;
        this.errorDayMsg = "Must be a valid day";
        error = true;
      }

      if (parseInt(month) < 0 || parseInt(month) > 12) {
        this.errorMonth = true;
        this.errorMonthMsg = "Must be a valid month";
        error = true;
      }

      if (parseInt(year) < 0 || parseInt(year) > today.getFullYear()) {
        this.errorYear = true;
        this.errorYearMsg = "Must be in the past";
        error = true;
      }

      return error;
    },
  },
};
</script>

<style scoped>
.box {
  /* border: 1px solid black; */
  border-radius: 20px;
  border-bottom-right-radius: 200px;
  padding: 3rem;
  background-color: white;
  box-shadow: 2px 1px 500px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.input-box {
  display: flex;
  flex-direction: column;
  text-align: left;
}

input {
  padding: 0.75rem 1rem;
  font-size: 24px;
  font-weight: bold;
  border: 1px solid hsl(0, 0%, 94%);
  border-radius: 5px;
  width: 100px;
  /* margin-right: 1rem; */
}

input:focus {
  outline: none !important;
  border: 1px solid hsl(259, 100%, 65%);
}

.input-error {
  border: 1px solid hsl(0, 100%, 67%);
}

label {
  color: hsl(0, 1%, 44%);
  font-weight: bold;
  margin-bottom: 0.5rem;
  font-size: 12px;
}

.label-error {
  color: hsl(0, 100%, 67%);
}

.form-container {
  position: relative;
  padding: 2rem 2rem 2.5rem 0;
  border-bottom: 1px solid hsl(0, 0%, 94%);
  margin-bottom: 2rem;
  width: 500px;
}

.form {
  display: flex;
  gap: 2rem;
}

.output {
  font-weight: 900;
  font-size: 72px;
  font-style: italic;
  line-height: 80px;
}

.number {
  color: hsl(259, 100%, 65%);
}

.icon-arrow {
  background-color: hsl(259, 100%, 65%);
  border-radius: 100%;
  padding: 1rem;
  position: absolute;
  right: -80px;
  bottom: -30px;
}

.icon-arrow:hover {
  background-color: hsl(0, 0%, 8%);
  border-color: hsl(0, 0%, 8%);
}

.error {
  color: hsl(0, 100%, 67%);
  font-size: 12px;
}

.attribution {
  margin-top: 2rem;
  font-size: 16px;
  text-align: center;
}
.attribution a {
  color: hsl(259, 100%, 65%);
}

.attribution a:hover {
  text-decoration: underline;
}
</style>
