<template lang="html">
  <div class="calculator">
    <h1>Weight Calculator</h1>
    <!-- <router-link @click="reset()" to="/">Clear</router-link>-->
    <!-- <router-link to="/">Clear</router-link> -->
    <div>
    <table>
      <thead>
        <tr>
          <th></th><th>Kg</th><th>Stone</th><th>Pounds</th>
        </tr>
      </thead>
      <tbody>
      <tr>
        <tr v-for="(form, index) in forms" :key="index">
           <td><label for="kg">weight{{ index+1}}  </label>
           </td>
           <td>
           <input v-on:change="kgConvert(index)" type="number"placeholder="kg" class="kg" name="kg" v-model="form.kg"></td>
           <td>
           <input v-on:change="stConvert(index)" type="number" placeholder="stone" class="st" name="st" v-model="form.st"></td>
           <td>
           <input v-on:change="lbsConvert(index)" type="number" placeholder="lbs" class="lbs" name="lbs" v-model="form.lbs"></td>
        </tr>
      </tbody>
    </table>
  </div>
    <div v-if="forms.length < 2" class="button">
      <button type="button" @click="addWeights">Compare to Another Weight</button>
    </div>
    <div v-if="percentage" class="percentage">
      <h4>Change: {{ kgDiff }}kg, {{ stDiff }}st, {{ lbsDiff }}lbs</h4>
      <h4>{{ percentage }}% <span v-if="increase">increase</span> <span v-else>decrease</span> </h4>
    </div>
    <div class="return">
      <a href="https://github.com/shanegibney/vue-weight-calculator">Return to Repo</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data(){
    return {
      msg: 'Go forth and calculate!',
      percentage: false,
      increase: null,
      kgDiff: null,
      stDiff: null,
      lbsDiff: null,
      forms: [{
        kg: null,
        st: null,
        lbs: null
      },
      {
        kg: null,
        st: null,
        lbs: null
      }]
    }
  },
  methods: {
    reset(){
      console.log("reset()");
      this.percentage = false,
      this.increase = null,
      this.kgDiff = null,
      this.stDiff = null,
      this.lbsDiff = null,
      this.forms = [{
        kg: null,
        st: null,
        lbs: null
      },
      {
        kg: null,
        st: null,
        lbs: null
      }]
    },
    // addWeights(){
    //   this.forms.push({
    //     kg: null,
    //     st: null,
    //     lbs: null
    //   })
    // },
    kgConvert(index){
      this.forms[index].st = (this.forms[index].kg * 0.157473107407).toFixed(2)
      this.forms[index].lbs = (this.forms[index].kg * (1/0.453592)).toFixed(2)
      this.makeCompare(index)
    },
    stConvert(index){
      this.forms[index].kg = (this.forms[index].st * (1/0.157473107407)).toFixed(2)
      this.forms[index].lbs = (this.forms[index].st * (1/0.0714286)).toFixed(2)
      this.makeCompare(index)
    },
    lbsConvert(index){
      this.forms[index].st = (this.forms[index].lbs * 0.0714286).toFixed(2)
      this.forms[index].kg = (this.forms[index].lbs * 0.453592).toFixed(2)
      this.makeCompare(index)
    },
    makeCompare(index){
      if(this.forms.length > 1){
         this.percentage = Math.abs(((this.forms[0].kg - this.forms[1].kg)/this.forms[0].kg)*100).toFixed(2)
         if(this.forms[0].kg > this.forms[1].kg){
           this.increase = false
         } else {
           this.increase = true
         }
         this.kgDiff = Math.abs((this.forms[0].kg - this.forms[1].kg).toFixed(2))
         this.stDiff = Math.abs((this.forms[0].st - this.forms[1].st).toFixed(2))
         this.lbsDiff = Math.abs((this.forms[0].lbs - this.forms[1].lbs).toFixed(2))
      }
    }
  }
}
</script>

<style lang="css">

.calculator {
  background: lightblue;
  max-width: 50%;
  margin-left: auto;
  margin-right: auto;
  border: 2px solid gray;
  border-radius: 7px;
}
input {
  max-width: 50px;
}
label {
  font-weight: bold;
  text-decoration: underline;
}
table {
  padding: 10px;
  padding-left: 50px;
  padding-right: 50px;
  border: 2px solid gray;
  border-radius: 7px;
  background: lightgray;
  margin-left: auto;
  margin-right: auto;
}
h1 {
  text-decoration: underline;
}
</style>
