<script setup>
import { ref } from "vue";

// getting input from the user
const day = ref("");
const month = ref("");
const year = ref("");

// Current date
const currentDate = new Date();
const currentYear = currentDate.getFullYear();
const currentMonth = currentDate.getMonth();
const currentDay = currentDate.getDate();

// Validation error
function validationError() {
  // empty fields variables
  let emptyDay = false;
  let emptyMonth = false;
  let emptyYear = false;
  // invalid numbers variables
  let invalidPast = false;
  let invalidMonth = false;
  let invalidDay = false;

  // last day of the birth month
  const lastDayOfMonth = new Date(
    parseInt(year.value),
    parseInt(month.value),
    0
  );

  // checks for the empty fields
  if (day.value == "") {
    emptyDay = true;
  }
  if (month.value == "") {
    emptyMonth = true;
  }
  if (year.value == "") {
    emptyYear = true;
  }
  // checks for the invalid numbers
  if (year.value > currentYear) {
    invalidPast = true;
  }
  if (month.value < 1 || month.value > 12) {
    invalidMonth = true;
  }
  if (day.value < 1 || day.value > lastDayOfMonth.getDate()) {
    invalidDay = true;
  }

  return {
    emptyDay,
    emptyMonth,
    emptyYear,
    invalidDay,
    invalidMonth,
    invalidPast,
  };
}

// Calculate age
function calculateAge() {
  // Calculate the differences between the birthdate and current date
  let diffYears = currentYear - parseInt(year.value);
  let diffMonths = currentMonth - parseInt(month.value);
  let diffDays = currentDay - parseInt(day.value);

  // If the current month and day are earlier than the birth month and day,
  if (diffMonths < 0 || (diffMonths == 0 && diffDays < 0)) {
    // Adjust the difference in years
    diffYears--;
    // Add 12 to the difference in months to get a positive value
    diffMonths = 12 + diffMonths;
  }
  
  // If the current day is positive and earlier than the birth day, increment the month difference
  if (diffMonths <= 12 && diffDays >= 0) {
    diffMonths++;
  }
  
  // If the current days are negative or earlier than the birth day, adjust the difference
  if (diffDays < 0) {
    // Get the last day of the birth month
    const lastDayOfMonth = new Date(
      parseInt(year.value),
      parseInt(month.value),
      0
      );
      // Add the last day of the birth month to the difference to get a positive value
      diffDays = lastDayOfMonth.getDate() + diffDays;
    }
    
    // If the month is 12, represent it as 0 (January of the following year)
    if (diffMonths == 12) {
      diffMonths = 0;
    }
    // If the months are same and the days same or more
    if(diffMonths == 0 && diffDays >= 0){
      diffYears++;
    }
  
  // Return the calculated age as an object containing years, months, and days
  return { years: diffYears, months: diffMonths, days: diffDays };
}

// Zodiac sign
function zodiac() {
  // Conditions for getting the right zodiac
  if (
    (month.value == 3 && day.value >= 21) ||
    (month.value == 4 && day.value <= 19)
  ) {
    zodiacSign.value = "Aries";
  } else if (
    (month.value == 4 && day.value >= 20) ||
    (month.value == 5 && day.value <= 20)
  ) {
    zodiacSign.value = "Taurus";
  } else if (
    (month.value == 5 && day.value >= 21) ||
    (month.value == 6 && day.value <= 20)
  ) {
    zodiacSign.value = "Gemini";
  } else if (
    (month.value == 6 && day.value >= 21) ||
    (month.value == 7 && day.value <= 22)
  ) {
    zodiacSign.value = "Cancer";
  } else if (
    (month.value == 7 && day.value >= 23) ||
    (month.value == 8 && day.value <= 22)
  ) {
    zodiacSign.value = "Leo";
  } else if (
    (month.value == 8 && day.value >= 23) ||
    (month.value == 9 && day.value <= 22)
  ) {
    zodiacSign.value = "Virgo";
  } else if (
    (month.value == 9 && day.value >= 23) ||
    (month.value == 10 && day.value <= 22)
  ) {
    zodiacSign.value = "Libra";
  } else if (
    (month.value == 10 && day.value >= 23) ||
    (month.value == 11 && day.value <= 21)
  ) {
    zodiacSign.value = "Scorpio";
  } else if (
    (month.value == 11 && day.value >= 22) ||
    (month.value == 12 && day.value <= 21)
  ) {
    zodiacSign.value = "Sagittarius";
  } else if (
    (month.value == 12 && day.value >= 22) ||
    (month.value == 1 && day.value <= 19)
  ) {
    zodiacSign.value = "Capricorn";
  } else if (
    (month.value == 1 && day.value >= 20) ||
    (month.value == 2 && day.value <= 18)
  ) {
    zodiacSign.value = "Aquarius";
  } else if (
    (month.value == 2 && day.value >= 19) ||
    (month.value == 3 && day.value <= 20)
  ) {
    zodiacSign.value = "Pisces";
  }
}
// Chinese zodiac calculation
function chineseZodiac(birthYear) {
  const animals = [
    "Rat",
    "Ox",
    "Tiger",
    "Rabbit",
    "Dragon",
    "Snake",
    "Horse",
    "Goat",
    "Monkey",
    "Rooster",
    "Dog",
    "Pig",
  ];
  const startYear = 1900; // Starting year of the Chinese zodiac cycle

  // Calculate offset from the starting year to the given birth year
  const offset = (birthYear - startYear) % 12;

  // Determine the animal based on the offset
  const animalIndex = offset >= 0 ? offset : 12 + offset;

  // Return the corresponding animal
  animalSign = animals[animalIndex];
}

// Initial value for the results
let validation = ref("");
let age = ref({ years: "--", months: "--", days: "--" });
let zodiacSign = ref("--");
let animalSign = ref("--");

// Submission function
function calculate() {
  // if there's no error then it will show the age, and the zodiacs
  validation.value = validationError();
  if (
    validation.value.emptyDay == false &&
    validation.value.emptyMonth == false &&
    validation.value.emptyYear == false &&
    validation.value.invalidDay == false &&
    validation.value.invalidMonth == false &&
    validation.value.invalidPast == false
  ) {
    age = calculateAge();
    zodiac();
    chineseZodiac(year.value);
  }
}
</script>

<template>
  <main class="mb-5">
    <dev class="interface">
      <form action="#" method="post">
        <div class="inputs row mb-3">
          <!-- the :class only applies the css if the boolean inside is true-->
          <!-- the @keyup.enter is used as an event handler for enter, it can only be used in input fields-->
          <div class="day col-4 col-md-3">
            <label
              for="day"
              class="form-label"
              :class="{
                'text-danger': validation.emptyDay || validation.invalidDay,
              }"
              >DAY</label
            >
            <input
              v-model="day"
              id="day"
              type="text"
              class="form-control"
              placeholder="DD"
              @keyup.enter.prevent="calculate()"
              :class="{
                'is-invalid': validation.emptyDay || validation.invalidDay,
              }"
            />
            <p v-if="validation.emptyDay" class="validationMessage">
              This field is required
            </p>
            <p v-else-if="validation.invalidDay" class="validationMessage">
              Must be a valid day
            </p>
          </div>
          <div class="month col-4 col-md-3">
            <label
              for="month"
              class="form-label"
              :class="{
                'text-danger': validation.emptyMonth || validation.invalidMonth,
              }"
              >MONTH</label
            >
            <input
              v-model="month"
              id="month"
              type="text"
              class="form-control"
              placeholder="MM"
              @keyup.enter.prevent="calculate()"
              :class="{
                'is-invalid': validation.emptyMonth || validation.invalidMonth,
              }"
            />
            <p v-if="validation.emptyMonth" class="validationMessage">
              This field is required
            </p>
            <p v-else-if="validation.invalidMonth" class="validationMessage">
              Must be a valid month
            </p>
          </div>
          <div class="year col-4 col-md-3">
            <label
              for="year"
              class="form-label"
              :class="{
                'text-danger': validation.emptyYear || validation.invalidYear,
              }"
              >YEAR</label
            >
            <input
              v-model="year"
              id="year"
              type="text"
              class="form-control"
              placeholder="YYYY"
              @keyup.enter.prevent="calculate()"
              :class="{
                'is-invalid': validation.emptyYear || validation.invalidPast,
              }"
            />
            <p v-if="validation.emptyYear" class="validationMessage">
              This field is required
            </p>
            <p v-else-if="validation.invalidPast" class="validationMessage">
              Must be in the past
            </p>
          </div>
        </div>
        <div class="submission row-lg d-flex align-items-center">
          <hr class="col-md-10" />
          <div class="col-md-2 circle">
            <!-- the prevent is used to not let it reload the page -->
            <img
              class="img-fluid"
              src="./assets/images/icon-arrow.svg"
              alt="icon-arrow"
              @click.prevent="calculate()"
            />
          </div>
        </div>
      </form>
      <div class="result mb-5">
        <div class="years">
          <span>{{ age.years }}</span> years
        </div>
        <div class="months">
          <span>{{ age.months }}</span> months
        </div>
        <div class="days">
          <span>{{ age.days }}</span> days
        </div>
        <div class="zodiac">
          Zodiac: <span>{{ zodiacSign }}</span>
        </div>
        <div class="zodiac">
          Chinese Zodiac: <span>{{ animalSign }}</span>
        </div>
      </div>
    </dev>
  </main>
  <footer class="attribution">Coded by <a href="#">Sushmoy</a>.</footer>
</template>

<style scoped>
.attribution {
  text-align: center;
}
/* the position relative is used to use the z-index on it's child */
.submission {
  position: relative;
}

.circle {
  z-index: 1;
}

.circle img {
  background-color: hsl(259, 100%, 65%);
  border-radius: 50%;
  padding: 1rem;
  margin-left: -1rem;
  margin-top: -0.5rem;
}
.circle img:hover {
  background-color: hsl(0, 0%, 8%);
  cursor: pointer;
}

.result {
  font-style: italic;
  font-size: 4rem;
  line-height: 1.2;
  font-weight: 800;
}

.result span {
  color: hsl(259, 100%, 65%);
}

main {
  background-color: white;
  width: 100%;
  max-width: 620px;
  padding: 3rem;
  border-radius: 1rem;
  border-bottom-right-radius: 10rem;
}

.validationMessage {
  margin-top: 0.2rem;
  margin-bottom: -1rem;
  font-size: 0.7rem;
  font-style: italic;
  color: hsl(0, 100%, 67%);
}

.form-label {
  font-size: small;
  font-weight: 700;
  letter-spacing: 0.1rem;
  margin-bottom: 0.3rem;
  color: hsl(0, 1%, 44%);
}
.zodiac {
  font-size: 1.1rem;
  font-style: normal;
  font-weight: 700;
  margin: 2.5rem auto;
  line-height: 0;
}

@media (max-width: 768px) {
  .submission {
    padding: 1rem 0 0;
    margin: 1rem 0 2.5rem;
  }

  hr {
    width: 100%;
    border-top: 0.1rem solid black;
  }

  /* positions the arrow in the middle of the screen */
  .circle {
    position: absolute;
    left: 45.5%;
    top: 35%;
  }
  .result {
    font-size: 2.8rem;
  }
  main{
    padding: 1.5rem;
    border-bottom-right-radius: 8rem;
  }
  .zodiac{
    font-size: 1rem;
    margin: 2rem auto;
  }

}

</style>
