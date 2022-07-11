<template>
  <div class="container">
    <header>
      <h1>Simple, traffic-based pricing</h1>
      <p>Sign-up for our 30-day trial. No credit card required.</p> 
    </header>

    <div id="main">
      <div>
        <div class="price-slider">
          <div class="pageviews">{{pageViews}} pageviews</div>
          <div class="views-slider">
            <input type="range" :min="min" :max="max" :step="step"  v-model="sliderValue" @input="incrementSlider">
          </div>
          <div class="price"><span>${{sliderValue}}.00</span>/ month</div>
        </div>
        <div class="billing-toggle">
          <p>Monthy Billing</p>
          <label class="toggle">
            <input type="checkbox" v-model="yearly">
            <span class="switch"></span>
          </label>
          <p>Yearly Billing <span>25% <span>discount</span></span></p>
        </div>
      </div>

      <div>
        <div class="benefits">
          <div class="benefit">
            <span class="checkmark"><svg xmlns="http://www.w3.org/2000/svg" width="9" height="8"><path fill="none" stroke="#10D8C4" stroke-width="2" d="M1 4.134l1.907 1.908L7.949 1"/></svg>
            </span>
            Unlimited websites
          </div>
          <div class="benefit">
            <span class="checkmark">
              <svg xmlns="http://www.w3.org/2000/svg" width="9" height="8"><path fill="none" stroke="#10D8C4" stroke-width="2" d="M1 4.134l1.907 1.908L7.949 1"/></svg>
            </span>
            100% data ownership
          </div>
          <div class="benefit">
            <span class="checkmark">
              <svg xmlns="http://www.w3.org/2000/svg" width="9" height="8"><path fill="none" stroke="#10D8C4" stroke-width="2" d="M1 4.134l1.907 1.908L7.949 1"/></svg>
            </span>
            Email reports
          </div>
        </div>
        <button>Start my trial</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      sliderValue:16,
      pageViews:"10K",
      yearly:false,
      step:4,
      min:8,
      max:36,
    }
  },
  methods:{
    incrementSlider(event){
      if (this.yearly){
        if (event.target.value > 216){
        this.sliderValue=324;
        } else if(event.target.value >144){
        this.sliderValue= 216;
        }
      }else{
        if (event.target.value > 24){
        this.sliderValue=36;
        } else if(event.target.value >16){
        this.sliderValue= 24;
        }
      }
      
      if((this.sliderValue >=324)||(!this.yearly && this.sliderValue >= 36)){
        this.pageViews = "1M";
      }else if((this.sliderValue >=216) ||(!this.yearly && this.sliderValue >= 24)){
        this.pageViews = "500K";
      }else if((this.sliderValue >=144) ||(!this.yearly && this.sliderValue >= 16)){
        this.pageViews = "100K";
      }else if((this.sliderValue >=108) ||(!this.yearly && this.sliderValue >= 12)){
        this.pageViews = "50K";
      }else{
        this.pageViews = "10K";
      }
      
    }
  },
  watch:{
    sliderValue(){
      const value = ((this.sliderValue - this.min)/ (this.max - this.min))*100;
      document.documentElement.style.setProperty('--sliderPercent', `${value}%`);
    },
    yearly(){
      if(this.yearly){
        this.min=72;
        this.max=324;
        this.step= this.step*9;
        this.sliderValue = this.sliderValue * 0.75*12;
      } else{
        this.min=8;
        this.max=36;
        this.step=4;
        this.sliderValue = this.sliderValue / (0.75*12) ;
      }
    }
  }

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;800&display=swap');

:root{
  --sliderPercent: 28.571%;
}

#app{
  min-height: 100vh;
  display:flex;
  justify-content: center;
  align-items: center;
}
.container{
  width:85%;
  max-width: 500px;
}

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Manrope', sans-serif;
  font-size: 15px;
}

body{
  background-image: url("./assets/bg-pattern.svg");
  background-color: #fafbff;
  background-repeat: no-repeat;
  font-weight: 600;
  color:#858fad;
}

header{
  padding:1.2rem;
  text-align: center;
  background-image: url("./assets/pattern-circles.svg");
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  position: relative;
  top:-20px;
}

h1{
  font-size:1.3rem;
  font-weight: 800;
  color:#293356;
  margin:1rem 0;
}

header >p {
  padding: 0 2rem;
}

#main{
  background-color: #fff;
  border-radius: 0.6rem;
  box-shadow: 5px 5px 20px 2px #cdd7ee;
}

#main>div{
  padding:1.5rem;
  text-align: center;
}

.views-slider{
  padding:1.5rem 0;
}

input[type=range] {
  -webkit-appearance: none; /* Hides the slider so that custom slider can be made */
  width: 100%; /* Specific width is required for Firefox. */
  background: linear-gradient(to right, #a5f3eb 0%, #a5f3eb var(--sliderPercent), #eaeefb var(--sliderPercent),#eaeefb 100%); /* Otherwise white in Chrome */
  border-radius: 10px;
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
}

input[type=range]:focus {
  outline: none; /* Removes the blue border. You should probably do some kind of focus styling for accessibility reasons though. */
}

/* Custom  slider styles */

input[type=range]::-webkit-slider-thumb{
  -webkit-appearance: none;
  height: 36px;
  width: 36px;
  border-radius: 50%;
  background-color: #10d5c2;
  background-image: url("./assets/icon-slider.svg");
  background-repeat: no-repeat;
  background-position: center;
  cursor: pointer;
  margin-top: -13px; /* You need to specify a margin in Chrome, but in Firefox and IE it is automatic */ 
  box-shadow: 0px 7px 25px 3px #10d5c2;
}

input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 10px;
  cursor: pointer;
  border-radius: 10px;
}

.pageviews{
  text-transform: uppercase;
  letter-spacing: 0.2rem;
}

.price>span{
  font-size: 2rem;
  font-weight: 800;
  color:#293356;
  margin-right: 0.5rem;
}

.billing-toggle{
  display: flex;
  justify-content: right;
  margin-top:1rem;
  padding:1rem 0;
  gap:1rem;
  align-items: center;

}

.billing-toggle *{
  font-size: 0.8rem;
}

.billing-toggle p>span{
  color:hsl(15, 100%, 70%);
  background-color: hsl(14, 92%, 95%);
}

.billing-toggle p>span>span{
  display: none;
}

.toggle{
  position: relative;
  width:48px !important;
  min-width: 48px;
  height: 28px;
  display: inline-block;
}

.toggle input{
  width:0;
  height:0;
  opacity: 0;
}

.switch{
  position: absolute;
  top:0;
  bottom:0;
  left:0;
  right:0;
  width:100%;
  border-radius: 28px;
  background-color: #cdd7ee;
  border: none;
  cursor: pointer;
  transition: .4s;
}

.switch::before{
  content:'';
  width:20px;
  height:20px;
  background-color: #fff;
  border-radius: 50%;
  position: absolute;
  left:4px;
  top:50%;
  transform: translate(0, -50%);
  transition: .4s;
}

.toggle input:hover + .switch{
  background-color: #10d5c2;
}

.toggle input:checked + .switch::before{
  transform: translate(20px, -50%);
  
}

.benefit{
  font-weight: 400;
  margin-bottom: 0.8rem;
}

button{
  background-color: #293356;
  color:#bdccff;
  border: none;
  padding:0.75rem 2rem;
  border-radius: 1.5rem;
  margin:1rem 0;
  cursor:pointer
}

button:hover{
  color:#eaeefb;
}

#main > div:last-of-type{
  border-top:1px solid #cdd7ee ;
}
@media (min-width:1025px){
.container{
  width:60%;
  max-width: 600px;
}

#main{
  border-radius: 1rem;
}

#main>div{
  padding: 2.5rem 3.5rem;
}

header{
  padding:2rem 1.2rem;
}

h1{
  font-size:2rem;
}

.price-slider{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}

.views-slider{
  width:100%;
  order:2;
  padding:3rem 0;
}

.price{
  order: 1;
}
.price>span{
  font-size: 2.8rem;
}

.billing-toggle{
  margin-top:0rem;
  padding:0;
  gap:1.3rem;
}

.billing-toggle > *{
  font-size: 1rem;
}

.billing-toggle p>span>span{
  display: inline;
}
#main > div:last-of-type{
  display: flex;
  justify-content: space-between;
}
.benefits{
  text-align: left;
}

button{
  padding:0.8rem 3rem;
  border-radius: 3rem;
}
}
</style>
