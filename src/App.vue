<template>
  <div id="app">
    <div class="container">
      <div class="header">
        Future Interest Calculation
      </div>
      <div>
        <div class="content">
          <div class="leftText">
            You Will Calculate
          </div>
          <div class="rightField">
            Future value and interest ammount
          </div>
        </div>
        <div class="content">
          <div class="leftText">
            Present value(Capital)
          </div>
          <div class="rightField">
            <label>
              <input v-model="capital" class="capitalInput">
            </label>
          </div>
        </div>
        <div class="content">
          <span class="leftText">Interest Rate</span>
          <label>
            <input v-model="rate" class="inputs">
          </label>
          <label>
            <select class="selectOption" v-model="rateType">
              <option value="Yearly" class="options">% Yearly</option>
              <option value="Monthly" class="options">% Monthly</option>
              <option value="Daily" class="options">% Daily</option>
            </select>
          </label>
        </div>
        <div class="content">
          <span class="leftText">Term</span>
          <label>
            <input v-model="termValue" class="inputs">
          </label>
          <label>
            <select v-model="term" class="selectOption">
              <option v-for="item in termArray" :value="item" :key="item">{{ item }}</option>
            </select>
          </label>
        </div>
        <div class="calculateArea">
          <button :style="[buttonDisable ? disabledStyle : '']" :disabled="buttonDisable" @click="interestCalculation()" class="calculateButton">Calculate</button>
        </div>
      </div>
    </div>
    <div class="calculateResult" v-if="result !==0">
      <span class="title">Calculation Result</span>
      <label class="outText">
        if you invested {{capital}}$ now, 
        {{termValue}} {{term}} later, you get <span class="resultText">{{result}}$</span>
      </label>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  
  data() {
    return {
      capital: null,
      rate: 0,
      rateType: 'Yearly',
      termValue: 0,
      term: 'Year',
      result: 0,
      termArray: [
        'Year',
        'Month',
        'Day',
      ],
      disabledStyle: {
        background: '#ccc',
      },
    }
  },
  methods: {
    interestCalculation() {
      const result = this.capital * Math.pow((1 + this.rate * 0.01), this.computedTermValue);
      this.result = parseInt(result);
      console.log(this.result)
    }
  },
  computed: {
    computedTermValue() {
      if (this.rateType === 'Yearly') {
        if (this.term === 'Year') {
          return this.termValue
        } else if (this.term === 'Month') {
          return this.termValue/12
        } else {
          return this.termValue/360
        }
      } else if (this.rateType === 'Monthly') {
        if (this.term === 'Year') {
          return this.termValue * 12
        } else if (this.term === 'Month') {
          return this.termValue
        } else return this.termValue/30
      } else {
          if (this.term === 'Year') {
          return this.termValue * 360
        } else if (this.term === 'Month') {
          return this.termValue * 30
        } else return this.termValue
      }
    },
    buttonDisable() {
      return this.termValue ==0 || this.rate ==0 || this.result ===null
    }
  }
}
</script>

<style>
.container {
  font-family: Arial, sans-serif;
  font-size: 15px;
  max-width: 600px;
  background-color: #f5f5f9;
  border-radius: 7px;
  box-shadow: 0 0 12px #888;
  margin-top: 20px;
  padding: 20px 10px 10px;
  margin-right: auto;
  margin-left: auto;
}

.header {
  font-weight: normal;
  padding-bottom: 5px;
  font-size: 17px;
  padding-top: 5px;
  width: 100%;
  text-shadow: 1px 1px 1px #fff;
  letter-spacing: 1px;
  text-align: center;
}

.content {
  margin-top: 20px;
  display: flex;
}

.leftText {
  width: 40%;
  align-items: center;
  justify-content: center;
  display: flex;
  font-size: 15px;
  color: #004372;
}

.rightField {
  font-size: 14px;
  width: 60%;
  display: flex;
  flex-direction: column;
}

.capitalInput {
  width: 95%;
  margin-top: 5px;
  margin-bottom: 5px;
  padding: 7px;
  border: solid 1px #e8ede8;
  border-radius: 3px;
  box-shadow: 0 0 2px silver;
  font-size: 14px;
  resize: vertical;
}

.inputs {
  margin-top: 5px;
  margin-bottom: 5px;
  padding: 7px;
  border-radius: 3px;
  border: solid 1px #e8ede8;
  box-shadow: 0 0 1px silver;
  font-size: 14px;
}

.selectOption {
  padding: 7px;
  border-radius: 3px;
  border: solid 1px #e8ede8;
  box-shadow: 0 0 1px silver;
  font-size: 12px;
  margin: 5px 4px 5px 56px;
  width: 115px;
}

.options {
  display: block;
  white-space: pre;
  min-height: 1.2em;
  padding: 0 2px 1px;
}

.calculateArea {
  border-top: 1px solid silver;
  display: flex;
  justify-content: flex-end;
  margin-top: 15px;
}

.calculateButton {
  box-sizing: content-box;
  padding: 10px;
  min-width: 100px;
  background-color: #0094ff;
  color: white;
  border-radius: 3px;
  cursor: pointer;
  border: solid 1px transparent;
  font-weight: 600;
  text-shadow: 1px 1px 1px #555;
  position: relative;
  margin-top: 30px;
}

.calculateResult {
  max-width: 600px;
  background-color: #d8dbd3;
  box-shadow: 0 0 12px #888;
  border-radius: 7px;
  margin-top: 20px;
  padding: 20px 10px 10px;
  margin-right: auto;
  margin-left: auto;
  display: flex;
  flex-direction: column;
}

.title {
  text-align: center;
  font-family: Cambria, sans-serif;
  border-bottom: 1px solid silver;
  padding-bottom: 10px;
  font-size: 20px;
  color: #23233b;
}

.outText {
  color: #313629;
  margin-top: 20px;
}

.resultText {
  font-weight: 600;
  font-size: 18px;
}

</style>
