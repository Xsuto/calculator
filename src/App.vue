<template>
  <main>
    <Frame/>
    <div class="output">
      <h1>{{output}}</h1>
    </div>
    <div class="grid">
      <button @click="clear" class="grey">C</button>
      <button @click="changeSing" class="grey">+/-</button>
      <button @click="operator('percentage')" :class="{active: wantedAction === 'percentage'}" class="grey">%</button>
      <button @click="operator('divide')" :class="{active: wantedAction === 'divide'}" class="orange">/</button>
      <button @click="addValue(7)">7</button>
      <button @click="addValue(8)">8</button>
      <button @click="addValue(9)">9</button>
      <button  @click="operator('multiply')" :class="{active: wantedAction === 'multiply'}" class="orange">*</button>
      <button @click="addValue(4)">4</button>
      <button @click="addValue(5)">5</button>
      <button @click="addValue(6)">6</button>
      <button @click="operator('subtract')" :class="{active: wantedAction === 'subtract'}" class="orange">-</button>
      <button @click="addValue(1)">1</button>
      <button @click="addValue(2)">2</button>
      <button @click="addValue(3)">3</button>
      <button @click="operator('add')" :class="{active: wantedAction === 'add'}" class="orange">+</button>
      <button @click="addValue(0)" class="zero-btn">0</button>
      <button @click="addValue('.')">.</button>
      <button @click="operator('equal')" class="orange">=</button>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent,ref } from "vue";
import Frame from '@/components/Frame.vue'

export default defineComponent({
  name: "App",
  components:{
    Frame
  },
  setup(){
    window.addEventListener('keydown',(e) => {
      if(isFinite(Number(e.key))){
        addValue(Number(e.key))
      }
      if(e.key === '+'){
        operator('add')
      }
      if(e.key === '-'){
        operator('subtract')
      }
      if(e.key === '*'){
        operator('multiply')
      }
      if(e.key === '/'){
        operator('divide')
      }
      if(e.key === '%'){
        operator('percentage')
      }
      if(e.key === 'Enter'){
        operator('equal')
      }
    })
    const output = ref("0")
    const tempValue = ref("0")
    const newOperation = ref(true)
    const wantedAction = ref('')
    const gotFinnalAnswer = ref(false)
    function isFloat(n: number){
    return Number(n) === n && n % 1 !== 0;
    }     
    const addValue = (value: number | string) => {
      console.log(value);
      
      if(gotFinnalAnswer.value && !wantedAction.value){
        clear()
        output.value = `${value}`
        gotFinnalAnswer.value = false
        newOperation.value = false
        return
      }
      if(gotFinnalAnswer.value && wantedAction.value) {
        tempValue.value = output.value
        output.value = `${value}`
        gotFinnalAnswer.value = false
        newOperation.value = false
        return
      }
      if(tempValue.value !== '0' && newOperation.value){
        output.value = `${value}`
        gotFinnalAnswer.value = false
        newOperation.value = false
        return
      }
      if((typeof value === "number" && output.value === "0")) {  
        output.value = `${value}`
        gotFinnalAnswer.value = false
        return
      }
      if((typeof value === "number" && output.value === "-0")) {  
        output.value = `-${value}`
        gotFinnalAnswer.value = false
        return
      }
      output.value += value
      gotFinnalAnswer.value = false
      newOperation.value = false
    }
    const clear = () => {
      output.value = '0'
      tempValue.value = '0'
      newOperation.value = true
      wantedAction.value = ''
      gotFinnalAnswer.value = false
    }
    const changeSing = () => {
      const sing = output.value.split('')[0]
      if(sing === '-'){
        output.value = output.value.split('').filter(char => char !== '-').join('')
      }else{
        output.value = '-' + output.value
      } 
    }
    const equal = () => {
        if(wantedAction.value === 'add') {
            let value = 0
            value += parseFloat(tempValue.value)
            value += parseFloat(output.value)
            output.value = isFloat(value) ? value.toFixed(5).toString() : value.toString() 
          }
          if(wantedAction.value === 'subtract') {
            let value = 0
            value += parseFloat(tempValue.value)
            value -= parseFloat(output.value)
            output.value = isFloat(value) ? value.toFixed(5).toString() : value.toString() 
          }
          if(wantedAction.value === 'multiply') {
            let value = 0
            value += parseFloat(tempValue.value)
            value *= parseFloat(output.value)    
            output.value = isFloat(value) ? value.toFixed(5).toString() : value.toString() 
          }
          if(wantedAction.value === 'divide') {
            let value = 0
            value += parseFloat(tempValue.value)
            value /= parseFloat(output.value)
            console.log(value.toString());
            output.value = isFloat(value) ? value.toFixed(5).toString() : value.toString() 
          }
        newOperation.value = true

        wantedAction.value = ''
        gotFinnalAnswer.value = true
    }
    const operator = (action: string) => {
      if(tempValue.value === '0'){
        if(action === 'percentage'){
            output.value = `${parseFloat(output.value) / 100}`
        }else{
          tempValue.value = output.value
          if(action === 'add') {
              wantedAction.value = 'add'
            }
          if(action === 'subtract') {
            wantedAction.value = 'subtract'  
            }
            if(action === 'multiply') {
            wantedAction.value = 'multiply'    
            }
            if(action === 'divide') {
              wantedAction.value = 'divide'
          }
          newOperation.value = true
        }
      }else{
        if(action === 'percentage'){
          output.value = `${(parseFloat(tempValue.value) / 100) * parseFloat(output.value)}`
        }else{
        equal()
        wantedAction.value = action
        }

      }
      if(action === 'equal') {
        equal()
        }
    }
    
    return {
      output,addValue,clear,operator,
      tempValue,newOperation,wantedAction,
      gotFinnalAnswer,changeSing
    }
  }
});
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300&display=swap');
*{
  font-family: 'Nunito', sans-serif;
  color: white;
}

body {
  margin: 0;
  background-color: rgba(0,0,0,0.3);
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
main {
  width: 100vw;
  min-height: 100vh;
}
button {
  border: none;
  border-left: 1px solid #000000;
  border-bottom: 1px solid #000000;
  outline: none;
  font-size: 2rem;
  background-color: #5E6064;
      &:active {
    filter: brightness(1.1);
  }
}

.output {
  min-width: 100vw;
  min-height: 20vh;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  background-color: rgba(0,0,0,0.3);
  -webkit-user-select: none;
  -webkit-app-region: drag;
  h1 {
    margin: 0 1rem 0 0;
    color: #FFFBEF;
    font-size: 3rem;
    
  }
}
.grid {
  width: 100vw;
  min-height: 75vh;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr
}

.orange {
  background-color: #FF9F06;
  border-right: 1px solid #A4ADAC;
  &:active {
    filter: brightness(0.7);
  }
}
.grey {
  background-color: #3E4047;
    &:active {
    filter: brightness(1.1);
  }
}
.active{
  background-color: #ffffff;
  color: #F5923D;
}
.zero-btn {
  grid-column-start: 1;
  grid-column-end: span 2;
}
</style>
