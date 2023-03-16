<script type="text/javascript">
import 'animate.css/animate.min.css'; 

export default{
    data(){

        let history = [];
        return {
            result: '',
            operation: '',
            history,
            iconClass: "",
            message: '',
            operators: ["-", "+", "/", "*"]
        }
    },
    methods: {
        handleButtonClick(event) {
            const buttonValue = event.target.value;
            if(buttonValue == "CE"){
               this.clearAll();
            } else if (this.operators.includes(buttonValue)){
                this.operator(buttonValue);
            } else if(buttonValue == "="){
                this.resultOperation();
            } else if(buttonValue == "C"){
                this.deleteOne(buttonValue);
            } else {
               this.executeOperation(buttonValue); 
            }
        },
        executeOperation(buttonValue){
            this.message = '';
            this.operation += buttonValue
            this.result = eval(this.operation);
        },
        operator(buttonValue){
            if(this.operation == '' && this.result != ''){
                    this.operation = (this.result + buttonValue)
            } else if (this.result == '' && this.operation == ''){
                this.message = "can not use an operator without a number"
            } else {
                if(this.verifyLastOperator()){
                    this.operation = this.operation.slice(0, -1) + buttonValue;
                }else{
                    this.operation += buttonValue;
                }
            }
        },
        resultOperation(){
            this.history.push({
                operationH: this.operation,
                resultH: this.result
            })
            this.operation = '';           
        },
        verifyLastOperator(){
            const lastChar = this.operation.slice(-1);
            const operatorsIndex = this.operators.indexOf(lastChar);
            return operatorsIndex !== -1;
        },
        deleteOne(){
            let deleteChar = this.operation.slice(0, -1);
            this.operation = deleteChar;
        },
        clearAll(){
            this.result = '';
            this.operation = '';
        },
        clearHistory(){
            this.history = [];
        },
        iconAnimation() {
            this.iconClass = "animate__animated animate__bounce";

            setTimeout(() => {
                this.iconClass = "";
            }, 3000);

        },
    },


}

</script>
<template>
    <div class="main-container">
        <div class="calculator-parent">

            <div class="offcanvas offcanvas-start" tabindex="-1" id="offcanvasExample" aria-labelledby="offcanvasExampleLabel" style="background-color: #221f1f ; color: white;">
                <div class="offcanvas-header">
                    <h5 class="offcanvas-title" id="offcanvasExampleLabel">History<img @click="clearHistory()" class="trashcan" src="../img/icons8-trash-can-50.png"></h5>
                    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
                </div>
                <div class="offcanvas-body">
                    <div  v-for="(item, index) in history" :key="index" class="history-element">
                        <p>Operation: {{ item.operationH }} | Result: {{ item.resultH }}</p>
                    </div>
                </div>
            </div>

            <div class="result-container">
                <p >{{ message }}</p>
                <h4  class="res">{{ operation }}</h4>
                <h2  class="res">{{ result }}</h2>
            </div>
            <div class="buttons-container">
                <button class="element o" value="CE" @click="handleButtonClick($event)">CE</button>
                <button class="element o" value="C" @click="handleButtonClick($event)">C</button>
                <button class="element" @click="iconAnimation()"><img :class="iconClass" src="../img/vue.png"></button>
                <button class="element" 
                        data-bs-toggle="offcanvas" 
                        data-bs-target="#offcanvasExample" 
                        aria-controls="offcanvasExample">
                        <img src="../img/icons8-h-67.png">
                </button>
                <button class="element" value="7" @click="handleButtonClick($event)">7</button>
                <button class="element" value="8" @click="handleButtonClick($event)">8</button>
                <button class="element" value="9" @click="handleButtonClick($event)">9</button>
                <button class="element operation o" value="*" @click="handleButtonClick($event)">x</button>
                <button class="element" value="4" @click="handleButtonClick($event)">4</button>
                <button class="element" value="5" @click="handleButtonClick($event)">5</button>
                <button class="element" value="6" @click="handleButtonClick($event)">6</button>
                <button class="element operation o" value="-" @click="handleButtonClick($event)">-</button>
                <button class="element" value="1" @click="handleButtonClick($event)">1</button>
                <button class="element" value="2" @click="handleButtonClick($event)">2</button>
                <button class="element" value="3" @click="handleButtonClick($event)">3</button>
                <button class="element operation o" value="+" @click="handleButtonClick($event)">+</button>
                <button class="element o" value="/" @click="handleButtonClick($event)">/</button>
                <button class="element" value="0" @click="handleButtonClick($event)">0</button>
                <button class="element operation o" value="." @click="handleButtonClick($event)">.</button>
                <button class="element operation e" value="=" @click="handleButtonClick($event)">=</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.trashcan{
    width: 20px;
    margin-left: 5px;
    margin-bottom: 5px;
}
.history-element{
    border-bottom: 1px solid #c5fc00;
    margin-bottom: 15px;
}
.o{
    background-color: #c5fc00;
}
.e{
    background-color: rgb(41, 41, 248);
    color: wheat;
}
h4.res{
    color: #555;
}
h2.res{
    color: #121212;
}
h4.res,
h2.res {
  white-space: nowrap;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch; /* para habilitar el scrolling en iOS */
}
h4.res::-webkit-scrollbar,
h2.res::-webkit-scrollbar {
  width: 8px;
  height: 3px;
}

h4.res::-webkit-scrollbar-track,
h2.res::-webkit-scrollbar-track {
  background-color: #f1f1f1;
}

h4.res::-webkit-scrollbar-thumb,
h2.res::-webkit-scrollbar-thumb {
  background-color: #000000;
  border-radius: 15px;
}

h4.res::-webkit-scrollbar-thumb:hover,
h2.res::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}
.main-container{
    display: grid;
    place-items: center;
    height: 100vh;
    background: rgb(186,172,172);
    background: linear-gradient(138deg, rgba(186,172,172,1) 24%, rgba(126,126,136,1) 49%, rgba(89,95,98,1) 75%);
}
.element{
    border: none;
    border-radius: 10px;
    display: grid;
    place-items: center;
    padding: 10px 0 10px 0;
    -webkit-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    -moz-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
}
.operation{
    font-size: 25px;
    font-weight: bold;
}
.calculator-parent {
    width: 35%;
    margin: 5px;
    padding: 20px;
    border: 2px solid rgb(46, 230, 0);
    background-color: #221f1f;
    border-radius: 15px;
    -webkit-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    -moz-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
}
.result-container{
    text-align: right;
    background-color: white;
    margin-bottom: 20px;
    border-radius: 15px;
    padding: 5px 10px;
    height: 120px;
    -webkit-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    -moz-box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
    box-shadow: 0px 0px 3px 0px rgba(0,0,0,0.75);
}
.buttons-container{
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-column-gap: 5px;
    grid-row-gap: 5px;
}
img{
    width: 30px;
}

@media screen and (max-width: 1000px){
    .calculator-parent{
        width: 60%;
    }
}

@media screen and (max-width: 600px){
    .calculator-parent{
        width: 80%;
    }
}
</style>