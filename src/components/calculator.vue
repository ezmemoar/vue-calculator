<template>
  <div class="calculator container">
    <div class="columns is-desktop is-vcentered has-full-height is-centered">
      <div class="card column is-3">
        <div class="card-content">
          <div class="content">

            <div class="columns">
              <div class="column">
                <div class="is-pulled-right">
                  <button class="button are-small is-grey-dark is-info">
                    history
                  </button>
                </div>
              </div>
            </div>

            <!-- for showing small history (not yet completed) -->
            <div class="columns">
              <div class="column has-background-white-bis">
                  <div class="is-pulled-right">
                    <!-- <span v-for="i in numberHistory.length" :key="i">{{ numberHistory[i].number }} {{ subtractorHistory[i].subtractor }} </span> -->
                    <!-- {{ numberHistory.length }} -->
                  </div>
              </div>
            </div>

            <div class="columns">
              <div class="main-text column is-12 has-text-black has-background-white-bis">
                {{ formattedNumber }}
                <div class="is-pulled-right">
                  {{ subtractor }}
                </div>
              </div>
            </div>

            <div class="has-text-centered">
              <div class="columns">
                <button @click="clear()" class="column is-3 button are-medium is-primary">C</button>
                <button @click="addPlusMinus()" class="column is-3 button are-medium is-link">+/-</button>
                <button @click="addPercent()" class="column is-3 button are-medium is-link">%</button>
                <button @click="deleteLast()" class="column is-3 button are-medium is-link">DEL</button>
              </div>
              <div class="columns">
                <button @click="addNumber('1')" class="column is-3 button are-medium is-info is-light">1</button>
                <button @click="addNumber('2')" class="column is-3 button are-medium is-info is-light">2</button>
                <button @click="addNumber('3')" class="column is-3 button are-medium is-info is-light">3</button>
                <button @click="addSubtractor('+')" class="column is-3 button are-medium is-success">+</button>
              </div>
              <div class="columns">
                <button @click="addNumber('4')" class="column is-3 button are-medium is-info is-light">4</button>
                <button @click="addNumber('5')" class="column is-3 button are-medium is-info is-light">5</button>
                <button @click="addNumber('6')" class="column is-3 button are-medium is-info is-light">6</button>
                <button @click="addSubtractor('x')" class="column is-3 button are-medium is-success">&times;</button>
              </div>
              <div class="columns">
                <button @click="addNumber('7')" class="column is-3 button are-medium is-info is-light">7</button>
                <button @click="addNumber('8')" class="column is-3 button are-medium is-info is-light">8</button>
                <button @click="addNumber('9')" class="column is-3 button are-medium is-info is-light">9</button>
                <button @click="addSubtractor('-')" class="column is-3 button are-medium is-success">-</button>
              </div>
              <div class="columns">
                <button @click="addNumber('0')" class="column is-3 button are-medium is-info is-light">0</button>
                <button @click="addDot()" class="column is-3 button are-medium is-info is-light">.</button>
                <button @click="showTotal()" class="column is-3 button are-medium is-danger">=</button>
                <button @click="addSubtractor('/')" class="column is-3 button are-medium is-success">/</button>
              </div>
            </div>
            <!-- {{ history }} -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data: function(){
    return{
      isSubExist: false,
      isHistoryAdded: false,
      number: "0",
      prevNumber: "",
      total: "",
      history: [],
      subtractor: "",
      iterate: 1
    }
  },
  computed: {
    formattedNumber(){
      return this.number.split(/(?=(?:...)*$)/).join('.');
    }
  },
  methods: {
    addNumber(params){
      if(this.number.charAt(0) == "0"){
        if(params != "0"){
          this.isHistoryExist();
          this.number = `${params}`;
        }
        if(this.isSubExist){
          this.isSubExist = false;
        }
      }
      else{
        if(!this.isSubExist){
          this.number += `${params}`;

          this.isHistoryExist();
        }
        else{
          this.number = `${params}`;
          this.isSubExist = false;

          this.isHistoryExist();
        }
      }
    },
    isHistoryExist(){
      if(this.isHistoryAdded == true){
        this.isHistoryAdded = false;
      }
    },
    addSubtractor(params){
      this.execTotal();
      this.subtractor = params;
      this.prevNumber = this.number;
      this.isSubExist = true;
    },
    clear(){
      this.number = "0";
      this.prevNumber = "";
      this.subtractor = "";
      this.history = [];
      this.iterate = 1;
      this.isHistoryAdded == false
    },
    execTotal(){
      if(this.isHistoryAdded == false && this.number && this.prevNumber){
        console.log(`${parseFloat(this.prevNumber)} ${parseFloat(this.number)}`);
        let total = "";
        if(this.subtractor == "/"){
          total = `${parseFloat(this.prevNumber) / parseFloat(this.number)}`;
        }
        else if(this.subtractor == "x"){
          total = `${parseFloat(this.prevNumber) * parseFloat(this.number)}`;
        }
        else if(this.subtractor == "+"){
          total = `${parseFloat(this.prevNumber) + parseFloat(this.number)}`;
        }
        else if(this.subtractor == "-"){
          total = `${parseFloat(this.prevNumber) - parseFloat(this.number)}`;
        }
  
        // append total first for adding to history
        this.total = total;      
        this.addToHistory();
        this.number = this.total;
        this.isSubExist = true;
      }
    },
    showTotal(){
      this.execTotal();

      // append any other history for showing to view and reset subtractor
      this.subtractor = "";
      this.prevNumber = "";
    },
    addToHistory(){
      this.history.push({
        id: this.iterate,
        process: `${this.prevNumber} ${this.subtractor} ${this.number}`,
        result: `${this.total}`
      });
      this.iterate += 1;
      this.isHistoryAdded = true;
    },
    addPlusMinus(){
      if(this.number.charAt(0) != "0"){
        if(this.number.charAt(0) === "-"){
          this.number = this.number.slice(1);
        }
        else{
          this.number = `-${this.number}`;
        }
      }
    },
    addPercent(){
      this.subtractor = "/";
      this.total = `${parseFloat(this.number) / 100}`;
      this.history.push({
        id: this.iterate,
        process: `${this.number} ${this.subtractor} 100`,
        result: `${this.total}`
      });

      // append to number for showing into front end
      this.number = `${parseFloat(this.number) / 100}`;
      this.iterate += 1;
    },
    deleteLast(){
      if(this.number.length != 1){
        this.number = this.number.slice(0, this.number.length - 1);
      }
      else{
        this.number = "0";
      }
    },
    addDot(){
      if(this.number.indexOf('.') == '-1'){
        this.addNumber('.');
      }
      else{
        this.number = this.number.slice(0, this.number.indexOf('.'));
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-text{
  font-size: 22px;
}
.button{
  border-radius: 0;
}
.column{
  height: auto;
}
</style>
