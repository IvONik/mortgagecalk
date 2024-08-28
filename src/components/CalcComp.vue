<template>
  <div>
    <h1>введите сумму кредита</h1>
    <input type="number" v-model="summ" step="10000" />
    <div class="slidecontainer">
      <input
        type="range"
        min="10000"
        max="20000000"
        step="10000"
        class="slider"
        id="mySumm"
        v-model="summ"
      />

      <div class="polz">
        <div
          role="button"
          class="number"
          style="left: 0%"
          @click="summ = 10000"
        >
          <span>10&nbsp;000 ₽</span>
        </div>
        <div
          role="button"
          class="number"
          style="left: 25%"
          @click="summ = 5000000"
        >
          <span>5&nbsp;млн ₽</span>
        </div>
        <div
          role="button"
          class="number"
          style="left: 50%"
          @click="summ = 10000000"
        >
          <span>10&nbsp;млн ₽</span>
        </div>
        <div
          role="button"
          class="number"
          style="right: 0%"
          @click="summ = 20000000"
        >
          <span>20&nbsp;млн ₽</span>
        </div>
      </div>
    </div>

    <h1>срок кредита <span>(в месяцах)</span></h1>
    <input type="number" v-model="period" />
    <div class="slidecontainer">
      <input
        type="range"
        min="3"
        max="120"
        step="1"
        class="slider"
        id="myPeriod"
        v-model="period"
      />
      <div class="polz">
        <div role="button" class="number" style="left: 0%" @click="period = 3">
          <span>3 мес</span>
        </div>
        <div
          role="button"
          class="number"
          style="left: 25%"
          @click="period = 24"
        >
          <span>2 года</span>
        </div>
        <div
          role="button"
          class="number"
          style="left: 50%"
          @click="period = 60"
        >
          <span>5 лет</span>
        </div>
        <div
          role="button"
          class="number"
          style="right: 0%"
          @click="period = 120"
        >
          <span>10 лет</span>
        </div>
      </div>
    </div>

    <h1>процентная ставка: {{ getPersent.persent }} %</h1>

    <div>
      <div class="box">
        <div>
          <input type="checkbox" id="card" name="card" v-model="isCard" />
          <label for="card"> есть зарплатная карта ( -0.4 % )</label>
        </div>
        <div>
          <input
            type="checkbox"
            id="history"
            name="history"
            v-model="isHistory"
          />
          <label for="history">
            есть положительная кредитная история ( -0.2 % )</label
          >
        </div>

        <div>
          <input
            type="checkbox"
            id="benefit"
            name="benefit"
            v-model="isBenefit"
          />
          <label for="benefit"> льготная категория ( -0.9 % )</label>
        </div>
      </div>
    </div>
    <hr />
    <p>ежемесячный платеж: {{ getPersent.payByMonth }} рублей</p>
    <p>общая сумма выплаты: {{ getPersent.total }} рублей</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      summ: 100000,
      defaultPersent: 14,
      period: 12,
      isCard: false,
      isHistory: false,
      isBenefit: false,
      person: {},
    };
  },
  computed: {
    getPersent() {
      let discount = 0;
      if (this.isCard) discount += 0.4;
      if (this.isHistory) discount += 0.2;
      if (this.isBenefit) discount += 0.9;

      const persent = (this.defaultPersent - discount).toFixed(2);
      const principal = parseFloat(this.summ);
      const interestRate = (this.defaultPersent - discount) / 100 / 12;
      const loanTermMonths = parseFloat(this.period);

      if (interestRate === 0) {
        const payByMonth = (principal / loanTermMonths).toFixed(2);
        const total = (payByMonth * loanTermMonths).toFixed(2);
        return { payByMonth, total, persent };
      }

      const numerator =
        principal * interestRate * Math.pow(1 + interestRate, loanTermMonths);
      const denominator = Math.pow(1 + interestRate, loanTermMonths) - 1;

      const payByMonth = (numerator / denominator).toFixed(2);
      const total = (payByMonth * loanTermMonths).toFixed(2);
      return { payByMonth, total, persent };
    },
  },
};
</script>

<style lang="css" scoped>
.box {
  display: flex;
  justify-content: center;
  align-items: start;
  flex-direction: column;
  align-content: flex-start;
}
.slider {
  margin-top: 16px;
  -webkit-appearance: none;
  width: 100%;
  height: 15px;
  border-radius: 5px;
  background: #d3d3d3;
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: #4caf50;
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: #4caf50;
  cursor: pointer;
}
.polz {
  display: flex;
  position: relative;
  margin-bottom: 60px;
}
.number {
  color: rgba(38, 38, 38, 0.7);
  cursor: pointer;
  padding-top: 9px;
  position: absolute;
  text-align: left;
}
</style>
