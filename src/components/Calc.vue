//<div @click="addOpp()" class="btn"></div>
<template>
  <div class="calc">
    <div @click="setBuffer(getBuffer())" class="display">
      {{ callDisplay() || "ERROR" }}
    </div>
    <div @click="addOpp('percent')" class="btn">%</div>
    <div @click="clearEntry" class="btn">CE</div>
    <div @click="clear" class="btn">C</div>
    <div @click="addOpp('pi')" class="btn">π</div>
    <div @click="del" class="btn">&larr;</div>
    <div @click="addOpp('oneOver')" class="btn">
      <sup>1</sup>&frasl;<sub>x</sub>
    </div>
    <div @click="addOpp('exp')" class="btn">x<sup>y</sup></div>
    <div @click="addOpp('root')" class="btn">&radic;</div>
    <div @click="addOpp('abs')" class="btn">|x|</div>
    <div @click="addOpp('div')" class="btn">&divide;</div>
    <div @click="addOpp('openP')" class="btn">(</div>
    <div @click="addOpp('closeP')" class="btn">)</div>
    <div @click="addOpp('n!')" class="btn"><em>n!</em></div>
    <div @click="addOpp('log')" class="btn">log</div>
    <div @click="addOpp('ln')" class="btn">ln</div>
    <div @click="num(7)" class="btn num">7</div>
    <div @click="num(8)" class="btn num">8</div>
    <div @click="num(9)" class="btn num">9</div>
    <div @click="addOpp('sin')" class="btn">sin()</div>
    <div @click="addOpp('mult')" class="btn">&times;</div>
    <div @click="num(4)" class="btn num">4</div>
    <div @click="num(5)" class="btn num">5</div>
    <div @click="num(6)" class="btn num">6</div>
    <div @click="addOpp('cos')" class="btn">cos()</div>
    <div @click="addOpp('sub')" class="btn">&minus;</div>
    <div @click="num(1)" class="btn num">1</div>
    <div @click="num(2)" class="btn num">2</div>
    <div @click="num(3)" class="btn num">3</div>
    <div @click="addOpp('tas')" class="btn">tas()</div>
    <div @click="addOpp('add')" class="btn">&#43;</div>
    <div @click="num(0)" class="btn num" id="zero">0</div>
    <div @click="addOpp('dot')" class="btn num">.</div>
    <div @click="addOpp('mod')" class="btn">Mod</div>
    <div @click="result()" class="btn">&crarr;</div>
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
      return this.display.join("");
    },
    //sets buffer to last display item
    //useful when deleting past current buffer
    redisplay() {
      this.display[this.display.length - 1] = this.getBuffer();
    },
    //returns current buffer
    getBuffer() {
      return parseFloat(this.buffer.join(""), 10);
    },
    //sets buffer with decimals and numbers
    setBuffer(num) {
      console.log(num);
      let b = String(num).split("");
      this.buffer = b.map((num) => {
        if (/[^.\D]/.test(num)) {
          return parseInt(num, 10);
        } else if (/\./.test(num)) {
          console.log("dot");
          return ".";
        } else {
          return num;
        }
      });
      console.log(`${this.buffer}`);
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
        this.buffer.pop;
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
          case 'openP':
            this.buffer.push('(');
            this.display.push('');
            this.buffer = [];
            break;
          case 'closeP':
            this.buffer.push(')');
            this.display.push('');
            this.buffer = [];
            break;
          case 'oneOver':
            this.setBuffer(1 / this.getBuffer());
            break;
          case 'n!': {
            let result = this.getBuffer();
            for (let i=this.getBuffer()-1; i>0; i--) {
              result *= i;
            }
            this.setBuffer(result);
            break;
          }
          case 'abs':
            this.setBuffer(Math.abs(this.getBuffer()));
            break;
          case 'sin':
          case 'cos':
          case 'tas':
          default:
            console.log('error');
            break;
        }
      }
    },
    //sets display, ans, and computes answer
    result() {
      let removeIndex = (arr, iArr) => {
        let res = [];
        for (let i = 0; i < arr.length; i++) {
          if (!iArr.includes(i)) res.push(arr[i]);
        }
        return res;
      };
      //PEMDAS math here
      let check = [/\(/, /\)/, /\^/, /√/, /^(x)/, /%/, /\//, /\+/, /-/];
      for (let x = 0; x < check.length; x++) {
        for (let i = 0; i < this.display.length; i++) {
          if (check[x].test(this.display[i])) {
            //if input is not a number do below in accordance with PEMDAS
            //compute prevNum and nextNum by opperator
            switch (x) {
              case 0:
              case 1:
                break;
              case 2:
                this.display[i - 1] = Math.pow(
                  this.display[i - 1],
                  this.display[i + 1]
                );
                break;
              case 3:
                this.display[i - 1] = Math.pow(
                  this.display[i - 1],
                  1 / this.display + 1
                );
                break;
              case 4:
                this.display[i - 1] = this.display[i - 1] * this.display[i + 1];
                break;
              case 5:
                this.display[i - 1] =
                  (this.display[i + 1] * this.display[i - 1]) / 100;
                break;
              case 6:
                this.display[i - 1] = this.display[i - 1] / this.display[i + 1];
                break;
              case 7:
                this.display[i - 1] = this.display[i - 1] + this.display[i + 1];
                break;
              case 8:
                this.display[i - 1] = this.display[i - 1] - this.display[i + 1];
                break;
            }
            this.display = removeIndex(this.display, [i, i + 1]);
          }
        }
      }
      this.setBuffer(this.display[this.display.length - 1]);
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
.num {
  background-color: grey;
  border-color: silver;
}
#zero {
  grid-column: 1/3;
}
</style>
