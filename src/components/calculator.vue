<template>
  <div class="calculator">
     <div class="content">{{content || 0}}</div>
     <button class="btn" @click="clear">C</button>
     <button class="btn" @click="sign">+/-</button>
     <button class="btn" @click="percent">%</button>
     <button class="btn operator" @click="divide">/</button>
     <button class="btn" @click="append('7')">7</button>
     <button class="btn" @click="append('8')">8</button>
     <button class="btn" @click="append('9')">9</button>
     <button class="btn operator" @click="multiply">x</button>
     <button class="btn" @click="append('4')">4</button>
     <button class="btn" @click="append('5')">5</button>
     <button class="btn" @click="append('6')">6</button>
     <button class="btn operator" @click="minus">-</button>
     <button class="btn" @click="append('1')">1</button>
     <button class="btn" @click="append('2')">2</button>
     <button class="btn" @click="append('3')">3</button>
     <button class="btn operator" @click="add">+</button>
     <button class="zero btn" @click="append('0')">0</button>
     <button class="btn" @click="dot">.</button>
     <button class="btn operator" @click="equal">=</button>
  </div>

</template>

<script>
export default {
  
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'calculator',

  data() {
    return {
      a: null,
      b: null,
      operator: null,
      content: '',
      result: null,
      toBeReset: false
    }
  },

  methods: {
    clear() {
      this.content= '';
      this.allReset();
    },

    append(number) {
      if(this.toBeReset) {
        this.content = '';
        this.toBeReset = false;
      }
      
      this.content = `${this.content}${number}`;
    },

    percent() {
      this.content = `${parseFloat(this.content)/100}`;
    },

    sign() {
      this.content = this.content.charAt(0) === '-' ? this.content.slice(1) : `-${this.content}`;
    },

    dot() {
      if(this.content.indexOf('.') === -1) {
        this.append(".");
      }
    },

    allReset() {
      this.operator = null;
      this.a = null;
      this.b = null;
      this.toBeReset = false;
    },

    performOp() {
      if(!this.toBeReset) {
        if(this.operator) {
          this.b = parseFloat(this.content);
          this.a = this.operator(this.a, this.b);
          this.content = this.a;
          this.toBeReset = true;
        } else {
          this.a = parseFloat(this.content);
          this.toBeReset = true;
        }
      }
    },

    add() {
      this.performOp();
      this.operator = (a,b) => a+b;  
    }, 

    minus() {
      this.performOp();
      this.operator = (a,b) => a-b;
    },

    multiply() {
      this.performOp();
      this.operator = (a,b) => a*b;
    },

    divide() {
      this.performOp();
      this.operator = (a,b) => a/b;
    },

    equal() {
      this.content = this.operator(this.a, parseFloat(this.content));
      this.allReset();
    }

  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(5rem, auto);
    margin: 0 auto;
    width: 30rem;
    border-radius: 4rem;
    gap: 1rem;
    padding: 3rem;
    box-sizing: border-box;
    background-color: #273344;
    overflow: hidden;
    margin-top: 4rem;
  }

  .btn {
    background-color: #2f3b4c;
    border-radius: 100%;
    font-size: 2rem;
    color: white;
    box-shadow: 0.2rem 0.1rem 0.5rem 0.1rem #2c2b2b;
    border-width: 0;
    cursor:pointer;
  }

  .content {
    font-size: 4rem;
    grid-column: 1/5;
    color: white;
    overflow-x: scroll;
    text-align: end;
  }

  .content::-webkit-scrollbar {
    width:0;
  }

  .zero {
    grid-column: 1/3;
    display: block;
    border-radius: 5rem;
  }

  .operator {
    color: orange;
  }
</style>
