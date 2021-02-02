<template>
  <div class='calc'>
    <div @click='redisplay' class='display'>{{ display || 0 }}</div>
    <div @click='percent' class='btn'>%</div>
    <div @click='clearEntry' class='btn'>CE</div>
    <div @click='clear' class='btn'>C</div>
    <div @click='back' class='btn'>Back</div>
    <div @click='oneOver' class='btn'>1/x</div>
    <div @click='square' class='btn'>x^2</div>
    <div @click='squareRoot' class='btn'>2/-x</div>
    <div @click='divide' class='btn'>/</div>
    <div @click='num(7)' class='btn num'>7</div>
    <div @click='num(8)' class='btn num'>8</div>
    <div @click='num(9)' class='btn num'>9</div>
    <div @click='multiply' class='btn'>X</div>
    <div @click='num(4)' class='btn num'>4</div>
    <div @click='num(5)' class='btn num'>5</div>
    <div @click='num(6)' class='btn num'>6</div>
    <div @click='subtract' class='btn'>-</div>
    <div @click='num(1)' class='btn num'>1</div>
    <div @click='num(2)' class='btn num'>2</div>
    <div @click='num(3)' class='btn num'>3</div>
    <div @click='add' class='btn'>+</div>
    <div @click='num(0)' class='btn num' id='zero'>0</div>
    <div @click='decimal' class='btn num'>.</div>
    <div @click='equal' class='btn'>=</div> 
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: undefined,
      buffer: [],
      result: 0
    }
  },
  methods: {
    redisplay() {
      this.display = parseInt(this.buffer.join(''), '10');
    },
    clear() {
      this.result = 0;
      this.clearEntry();
    },
    clearEntry() {
      this.display = 0;
    },
    percent() {
      this.result = this.result / 100;
    },
    num(number) {
      this.buffer.push(number);
      this.redisplay();
    },
    back() {
      this.buffer.pop();
      this.redisplay();
    }
  }
}
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
