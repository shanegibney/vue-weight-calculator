<template lang="html">
  <div class="calculator">
    <h1>Weight Calculator</h1>
    <div v-for="(form, index) in forms" :key="index">
      <label for="kg">kg:</label>
      <input v-on:keyup="kgConvert(index)" type="number" class="kg" name="kg" v-model="form.kg">
      <label for="st">st:</label>
      <input v-on:keyup="stConvert(index)" type="number" class="st" name="st" v-model="form.st">
      <label for="lbs">lbs:</label>
      <input v-on:keyup="lbsConvert(index)" type="number" class="lbs" name="lbs" v-model="form.lbs">
    </div>
    <div v-if="forms.length < 2" class="button">
      <button type="button" @click="addWeights">Compare to Another Weight</button>
    </div>
    <div v-if="percentage" class="percentage">
      <h4>Change: {{ kgDiff}}kg, {{ stDiff}}st, {{ lbsDiff}}lbs</h4>
      <h4>{{ percentage }}% <span v-if="increase">increase</span> <span v-else>decrease</span> </h4>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data(){
    return {
      msg: 'Go forth and calculate!',
      percentage: null,
      increase: null,
      kgDiff: null,
      stDiff: null,
      lbsDiff: null,
      forms: [{
        kg: null,
        st: null,
        lbs: null
      }]
    }
  },
  methods: {
    addWeights(){
      console.log("addWeights");
      this.forms.push({
        kg: null,
        st: null,
        lbs: null
      })
    },
    kgConvert(index){
      console.log("kgConvert");
      // this.kg = this.kg + 5;
      this.forms[index].st = (this.forms[index].kg * 0.157473107407).toFixed(2)
      this.forms[index].lbs = (this.forms[index].kg * (1/0.453592)).toFixed(2)
      this.makeCompare(index)
    },
    stConvert(index){
      console.log("stConvert");
      // this.kg = this.kg + 5;
      this.forms[index].kg = (this.forms[index].st * (1/0.157473107407)).toFixed(2)
      this.forms[index].lbs = (this.forms[index].st * (1/0.0714286)).toFixed(2)
      this.makeCompare(index)
    },
    lbsConvert(index){
      console.log("lbsConvert");
      // this.kg = this.kg + 5;
      this.forms[index].st = (this.forms[index].lbs * 0.0714286).toFixed(2)
      this.forms[index].kg = (this.forms[index].lbs * 0.453592).toFixed(2)
      this.makeCompare(index)
    },
    makeCompare(index){
      console.log("makeCompare index:" + index);
      if(this.forms.length > 1){
         console.log("makeCompare two");
         this.percentage = Math.abs(((this.forms[0].kg - this.forms[1].kg)/this.forms[0].kg)*100).toFixed(2)
         if(this.forms[0].kg > this.forms[1].kg){
           this.increase = false
         } else {
           this.increase = true
         }
         this.kgDiff = (this.forms[0].kg - this.forms[1].kg).toFixed(2)
         this.stDiff = (this.forms[0].st - this.forms[1].st).toFixed(2)
         this.lbsDiff = (this.forms[0].lbs - this.forms[1].lbs).toFixed(2)
      }
    }
  }
}
</script>

<style lang="css">
</style>
