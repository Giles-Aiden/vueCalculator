<template>
  <div class="calc">
    <div @click="setBuffer(getBuffer())" class="display">
      {{ display.join('') }}
    </div>
    <div @click="addOpp('percent')" class="btn">%</div>
    <div @click="clearEntry" class="btn">CE</div>
    <div @click="clear" class="btn">C</div>
    <div @click="del" class="btn">&larr;</div>
    <div @click="addOpp('oneOver')" class="btn">
      <sup>1</sup>&frasl;<sub>x</sub>
    </div>
    <div @click="addOpp('exp')" class="btn">x<sup>y</sup></div>
    <div @click="addOpp('root')" class="btn">&radic;</div>
    <div @click="addOpp('div')" class="btn">&divide;</div>
    <div @click="num(7)" class="btn num">7</div>
    <div @click="num(8)" class="btn num">8</div>
    <div @click="num(9)" class="btn num">9</div>
    <div @click="addOpp('mult')" class="btn">&times;</div>
    <div @click="num(4)" class="btn num">4</div>
    <div @click="num(5)" class="btn num">5</div>
    <div @click="num(6)" class="btn num">6</div>
    <div @click="addOpp('sub')" class="btn">&minus;</div>
    <div @click="num(1)" class="btn num">1</div>
    <div @click="num(2)" class="btn num">2</div>
    <div @click="num(3)" class="btn num">3</div>
    <div @click="addOpp('add')" class="btn">&#43;</div>
    <div @click="num(0)" class="btn num" id="zero">0</div>
    <div @click="addOpp('dot')" class="btn num">.</div>
    <div @click="result" class="btn">&crarr;</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: [0],
      buffer: [0],
      ans: 0,
    };
  },
  methods: {
    redisplay() {
      this.display[this.display.length - 1] = this.getBuffer();
    },
    getBuffer() {
      return parseFloat(this.buffer.join(''), 10);
    },
    setBuffer(num) {
      console.log(num);
      let b = String(num).split('');
      this.buffer = b.map((num) => {
        if (isNaN(num)) {
          return '.';
        } else {
          return parseInt(num, 10);
        }
      });
      this.redisplay();
      console.log(this.buffer);
    },
    getResult() {
      return;
    },
    addOpp(opp) {
      if (this.display[this.display.length - 1] == '') {
        console.log('is NaN');
      } else
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
            this.display.push('X');
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
            this.display.push(`^`);
            this.display.push('');
            this.buffer = [];
            break;
          case 'root':
            if (this.buffer[0] == 0 || this.buffer[0] == '') break;
            this.display.push('√');
            this.display.push('');
            this.buffer = [];
            break;
          case 'oneOver':
            this.setBuffer(1 / this.getBuffer());
            break;
        }
    },
    clear() {
      this.ans = 0; //add prev result here;
      this.display = [0];
    },
    clearEntry() {
      this.display[this.display.length - 1] = 0;
      this.buffer = [0];
    },
    num(number) {
      if (this.buffer[0] == 0) this.buffer[0] = number;
      else this.buffer.push(number);
      this.redisplay();
    },
    del() {
      if (this.buffer.length > 1) this.buffer.pop();
      else if (this.buffer[0] != 0) this.setBuffer(0);
      else if (this.buffer[this.buffer.length - 1] == ".") {
        console.log('.');
        this.buffer.pop;
        this.buffer.pop;
      }
      else if (this.display[0] == 0) return;
      else {
        this.display.pop();
        this.display.pop();
        this.setBuffer(this.display[this.display.length - 1]);
      }
      this.redisplay();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calc {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  margin: auto;
  width: 80%;
  height: 100%;
}
.display {
  grid-column: 1/5;
  background-color: black;
  color: white;
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
