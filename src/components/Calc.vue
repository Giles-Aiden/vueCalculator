<template>
  <div class="calc">
    <div @click="setBuffer(getBuffer())" class="display">
      {{ callDisplay() || 'ERROR' }}
    </div>
    <button @click="addOpp('percent')" class="btn">%</button>
    <button @click="clearEntry" class="btn">CE</button>
    <button @click="clear" class="btn">C</button>
    <button @click="addOpp('pi')" class="btn">π</button>
    <button @click="del" class="btn">&larr;</button>
    <button @click="addOpp('oneOver')" class="btn">
      <sup>1</sup>&frasl;<sub>x</sub>
    </button>
    <button @click="addOpp('exp')" class="btn">x<sup>y</sup></button>
    <button @click="addOpp('root')" class="btn">&radic;</button>
    <button @click="addOpp('abs')" class="btn">|x|</button>
    <button @click="addOpp('button')" class="btn">&divide;</button>
    <button @click="addOpp('openP-')" class="btn">(</button>
    <button @click="addOpp('closeP-')" class="btn">)</button>
    <button @click="addOpp('n!')" class="btn"><em>n!</em></button>
    <button @click="addOpp('log')" class="btn">log</button>
    <button @click="addOpp('ln')" class="btn">ln</button>
    <button @click="num(7)" class="btn num">7</button>
    <button @click="num(8)" class="btn num">8</button>
    <button @click="num(9)" class="btn num">9</button>
    <button @click="addOpp('sin')" class="btn">sin</button>
    <button @click="addOpp('mult')" class="btn">&times;</button>
    <button @click="num(4)" class="btn num">4</button>
    <button @click="num(5)" class="btn num">5</button>
    <button @click="num(6)" class="btn num">6</button>
    <button @click="addOpp('cos')" class="btn">cos</button>
    <button @click="addOpp('sub')" class="btn">&minus;</button>
    <button @click="num(1)" class="btn num">1</button>
    <button @click="num(2)" class="btn num">2</button>
    <button @click="num(3)" class="btn num">3</button>
    <button @click="addOpp('tan')" class="btn">tan</button>
    <button @click="addOpp('add')" class="btn">&#43;</button>
    <button @click="num(0)" class="btn num" id="zero">0</button>
    <button @click="addOpp('dot')" class="btn num">.</button>
    <button @click="addOpp('mod')" class="btn">％mod</button>
    <button @click="displayResult" class="btn">&crarr;</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //Stores unsplit floats and opperators
      display: [0],
      //Stores current float
      buffer: [0],
      //Previous answer
      ans: 0,
    };
  },
  methods: {
    callDisplay() {
      return this.display.join('');
    },
    //sets buffer to last display item
    //useful when deleting past current buffer
    redisplay() {
      this.display[this.display.length - 1] = this.getBuffer();
    },
    //returns current buffer
    getBuffer() {
      return parseFloat(this.buffer.join(''), 10);
    },
    //sets buffer with decimals and numbers
    setBuffer(num) {
      let b = String(num).split('');
      this.buffer = b.map((num) => {
        if (/[^.\D]/.test(num)) {
          return parseInt(num, 10);
        } else if (/\./.test(num)) {
          return '.';
        } else {
          return num;
        }
      });
      this.redisplay();
    },
    //resets calc
    clear() {
      this.ans = 0; //add prev result here;
      this.display = [0];
      this.buffer = [0];
    },
    //clears current number
    clearEntry() {
      this.display[this.display.length - 1] = 0;
      this.buffer = [0];
    },
    displayError() {
      this.display = [undefined];
      this.setBuffer(undefined);
    },
    //adds number to buffer
    num(number) {
      if (this.buffer[0] == 0 && this.buffer.length == 1)
        this.buffer[0] = number;
      else this.buffer.push(number);
      this.redisplay();
    },
    //deletes numbers, opperators, and decimals
    del() {
      if (this.buffer.length > 1) this.buffer.pop();
      else if (this.buffer[0] != 0 && this.buffer.length == 1)
        this.setBuffer(0);
      else if (/\./.test(this.buffer[this.buffer.length - 1])) {
        this.buffer.pop();
        this.buffer.pop;
      } else if (this.display[0] == 0) return;
      else {
        this.display.pop();
        this.display.pop();
        this.setBuffer(this.display[this.display.length - 1]);
      }
      this.redisplay();
    },
    //adds opperator to display array and makes new item to edit
    addOpp(opp) {
      if (this.display[this.display.length - 1] != '') {
        switch (opp) {
          case 'percent':
            this.display.push('%');
            this.display.push('');
            this.buffer = [];
            break;
          case 'div':
            this.display.push('/');
            this.display.push('');
            this.buffer = [];
            break;
          case 'mult':
            this.display.push('x');
            this.display.push('');
            this.buffer = [];
            break;
          case 'sub':
            this.display.push('-');
            this.display.push('');
            this.buffer = [];
            break;
          case 'add':
            this.display.push('+');
            this.display.push('');
            this.buffer = [];
            break;
          case 'dot':
            this.buffer.push('.');
            break;
          case 'exp':
            this.display.push('^');
            this.display.push('');
            this.buffer = [];
            break;
          case 'root':
            if (this.display.length >= 3 || this.display[0] != 0) {
              this.display.push('√');
              this.display.push('');
              this.buffer = [];
            }
            break;
          case 'mod':
            this.display.push('％');
            this.display.push('');
            this.buffer = [];
            break;
          case 'openP':
            this.display.push('(');
            this.display.push('');
            this.buffer = [];
            break;
          case 'closeP':
            this.display.push(')');
            this.display.push('');
            this.buffer = [];
            break;
          case 'oneOver':
            this.setBuffer(1 / this.getBuffer());
            break;
          case 'n!': {
            let result = this.getBuffer();
            for (let i = this.getBuffer() - 1; i > 0; i--) {
              result *= i;
            }
            this.setBuffer(result);
            break;
          }
          case 'abs':
            this.setBuffer(Math.abs(this.getBuffer()));
            break;
          case 'log':
            this.setBuffer(Math.log10(this.getBuffer()));
            break;
          case 'ln':
            this.setBuffer(Math.log(this.getBuffer()));
            break;
          case 'sin':
            this.setBuffer(Math.sin(this.getBuffer()));
            break;
          case 'cos':
            this.setBuffer(Math.cos(this.getBuffer()));
            break;
          case 'tan':
            this.setBuffer(Math.tan(this.getBuffer()));
            break;
          default:
            console.error('Invalid Opp Type');
            break;
        }
      }
    },
    displayResult() {
      this.buffer = this.result(this.display);
      this.display = [this.getBuffer()];
      this.redisplay();
    },
    //sets display, ans, and computes answer
    result(inputArr) {
      console.log('input arr ' + inputArr);
      if (inputArr.length == 1) return inputArr;
      let removeIndex = (arr, iArr) => {
        let res = [];
        for (let i = 0; i < arr.length; i++) {
          if (!iArr.includes(i)) res.push(arr[i]);
        }
        return res;
      };
      //PEMDAS math here
      let check = [/\(/, /\)/, /\^/, /√/, /^(x)/, /%/, /\//, /％/, /\+/, /-/];
      for (let x = 0; x < check.length; x++) {
        for (let i = 0; i < inputArr.length; i++) {
          if (check[x].test(inputArr[i])) {
            //if input is not a number do below in accordance with PEMDAS
            //compute prevNum and nextNum by opperator
            //i = current opp  x = current regex check
            switch (x) {
              case 0:
                if (check[1].test(inputArr.slice(x + 2))) {
                  let b = inputArr.slice(
                    x + 2,
                    inputArr.findIndex((ele) => ele == ')')
                  );
                  let r = [];
                  for (
                    let y = x + 1;
                    y < inputArr.findIndex((ele) => ele == ')') + 1;
                    y++
                  )
                    r.push(y);
                  if (b.length > 1) {
                    inputArr[i - 1] *= this.result(b);
                  } else inputArr[i - 1] *= b[0]; 
                } else this.displayError();
                break;
              case 1:
                break;
              case 2:
                inputArr[i - 1] = Math.pow(inputArr[i - 1], inputArr[i + 1]);
                break;
              case 3:
                inputArr[i - 1] = Math.pow(inputArr[i - 1], 1 / inputArr + 1);
                break;
              case 4:
                inputArr[i - 1] = inputArr[i - 1] * inputArr[i + 1];
                break;
              case 5:
                inputArr[i - 1] = (inputArr[i + 1] * inputArr[i - 1]) / 100;
                break;
              case 6:
                inputArr[i - 1] = inputArr[i - 1] / inputArr[i + 1];
                break;
              case 7:
                inputArr[i - 1] = inputArr[i - 1] % inputArr[i + 1];
                break;
              case 8:
                inputArr[i - 1] = inputArr[i - 1] + inputArr[i + 1];
                break;
              case 9:
                inputArr[i - 1] = inputArr[i - 1] - inputArr[i + 1];
                break;
            }
            inputArr = removeIndex(inputArr, [i, i + 1]);
          }
        }
      }
      return inputArr;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calc {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-auto-rows: minmax(50px, auto);
  margin: auto;
  width: 80%;
  height: 100%;
}
.display {
  grid-column: 1/6;
  background-color: black;
  color: white;
  overflow-x: auto;
  border: 0.2em solid silver;
  font-size: 5vw;
}
.btn {
  background-color: tan;
  border: 0.5em groove tan;
  font-size: 3vw;
}
.btn:hover {
  background-color: beige;
}
.btn:active {
  color: white;
  background-color: silver;
}
.num {
  background-color: grey;
  border-color: silver;
}
#zero {
  grid-column: 1/3;
}
</style>
