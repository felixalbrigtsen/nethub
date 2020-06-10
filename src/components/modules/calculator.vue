<template>
  <div class="calculator">
      <div class="display col-1-4">{{(showResult ? result : (curNum ? curNum : prevNum)).toString().slice(0,8)}}</div>
      <div class="btn col-1 operator" @click="curNum=''; prevNum='0'; curOperator=null; showResult=false">C</div>
      <div class="btn col-2 operator" @click="negate()">+/-</div>
      <div class="btn col-3 operator" @click="curNum = parseInt(curNum)/100">%</div>
      <div class="btn col-4 operator" @click="setOperator('divide')">/</div>
      <div class="btn col-1 number" @click="appendNum($event.target.innerHTML)">7</div>
      <div class="btn col-2 number" @click="appendNum($event.target.innerHTML)">8</div>
      <div class="btn col-3 number" @click="appendNum($event.target.innerHTML)">9</div>
      <div class="btn col-4 operator" @click="setOperator('multiply')">*</div>
      <div class="btn col-1 number" @click="appendNum($event.target.innerHTML)">4</div>
      <div class="btn col-2 number" @click="appendNum($event.target.innerHTML)">5</div>
      <div class="btn col-3 number" @click="appendNum($event.target.innerHTML)">6</div>
      <div class="btn col-4 operator" @click="setOperator('subtract')">-</div>
      <div class="btn col-1 number" @click="appendNum($event.target.innerHTML)">1</div>
      <div class="btn col-2 number" @click="appendNum($event.target.innerHTML)">2</div>
      <div class="btn col-3 number" @click="appendNum($event.target.innerHTML)">3</div>
      <div class="btn col-4 operator" @click="setOperator('add')">+</div>
      <div class="btn col-1-2 number row-6" @click="appendNum($event.target.innerHTML)">0</div>
      <div class="btn col-3 number" @click="curNum += '.'">,</div>
      <div class="btn col-4 operator row-6" @click="calculate">=</div>
  
  </div>
</template>

<script>
export default {
    name: "ModuleCalculator",
    data() {
        return {
            prevNum: "0",
            curNum: "",
            curOperator: null,
            result: "",
            showResult: false
        }
    }, 
    methods: {
        calculate() {
            if (this.curOperator == null) { return }
            if (this.curNum == "") { return }
            if (this.showResult) { this.prevNum = this.result }
            switch(this.curOperator) {
                case "add":
                    this.result = parseInt(this.prevNum) + parseInt(this.curNum)
                    break
                case "subtract":
                    this.result = parseInt(this.prevNum) - parseInt(this.curNum)
                    break
                case "multiply":
                    this.result = parseInt(this.prevNum) * parseInt(this.curNum)
                    break
                case "divide":
                    if (this.curNum == 0) { return }
                    this.result = parseInt(this.prevNum) / parseInt(this.curNum)
                    break
            }
            this.showResult = true
        },
        appendNum(num) {
            if (this.showResult) {
                this.showResult = false
                this.prevNum = this.result
                this.curNum = ""
            }
            this.curNum = this.curNum.toString() + num.toString()
        },
        setOperator(op) {
            if (this.showResult) { this.curOperator == null; this.showResult = false; }
            if (this.curOperator != null) {
                this.calculate()
            } else {
                this.prevNum = this.curNum
                this.curNum = ""
            }
            this.curOperator = op
            console.log(this.prevNum,this.curNum,this.curOperator)
        },
        negate() {
            this.curNum = (parseInt(this.curNum)*(-1)).toString()
        }
    }
}
</script>

<style scoped>
.calculator {
    display: grid;
    grid-template-columns: repeat(4, minmax(1fr, 100px));
    grid-template-rows: repeat(6, 1fr);
    height: 100%;
    width: 100%;
    max-width: 700px;
    font-size: 24px;
    text-align: center;
    margin: auto;

    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
    
}
.display {
    font-size: 30px;
    display: table-cell;
    vertical-align: bottom;
}
.btn {
    background-color: #aaa;
    border: 1px solid #333;
}
.operator {
    background-color: #777;
}

.btn, .display {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}
.col-1-4 {
    grid-column: 1/5;
}
.col-1 {
    grid-column: 1/2;
}
.col-2 {
    grid-column: 2/3;
}
.col-3 {
    grid-column: 3/4;
}
.col-4 {
    grid-column: 4/5;
    background-color: orange;
}
.col-1-2 {
    grid-column: 1/3;
}

.col-1-2.row-6 {
    border-bottom-left-radius: 10px;
}
.col-4.row-6 {
    border-bottom-right-radius: 10px;
}
</style>