<template>
 <div class="container">
<div class="container vertical-divider">
 <div class='columns'>

<div class='column is-4 padded'>
 
 <div>
<iframe src="https://calendar.google.com/calendar/embed?showTitle=0&amp;showPrint=0&amp;showTabs=0&amp;showCalendars=0&amp;showTz=0&amp;height=300&amp;wkst=1&amp;bgcolor=%2366cccc&amp;src=5n7nkavicq7nanjhucr5qu4mjg%40group.calendar.google.com&amp;color=%00d1b2&amp;ctz=America%2FDenver"" style="width:100%; height: 300px;" frameborder="0" scrolling="no" align="middle"></iframe>
</div>
<div>
<br>
<label>Please check availability of the room in the calendar above, then choose nights you would like to stay: </label>
<br>
<br>
<div>

<HotelDatePicker @checkinchanged="setcheckin" @checkoutchanged="setcheckout" format="MM/DD/YYYY" DatePickerID="01" :class="['is-danger' ? stayError : '']"
		:startDate="startDate"
		:endDate="endDate"
		:minNights="minNights" 
    		:maxNights="maxNights"
    		:showCloseButton="showCloseButton"
		
/>
<span class="help is-danger" v-show="stayError">
 {{ stayError }}
 </span>
 
</div>

</div>

</div>


<div class='column is-4 padded'> 
 <div class='field'>
 <label>Name</label>
 <div class="control has-icons-left has-icons-right">
 <input class="input" type="text" placeholder="First and Last" v-model='name'>
 <span class="icon is-small is-left">
 <i class="fa fa-user"></i>
 </span>
 </div>
 </div>
 <div class="field">
 <label>Email</label>
 <div class="control has-icons-left has-icons-right">
 <input id="email" type="email" placeholder="Email address" v-model='email' v-validate="email" data-vv-rules="required|email" :class="['is-danger' ? emailError : '', 'input']">
 <span class="icon is-small is-left">
 <i class="fa fa-envelope"></i>
 </span>
<p v-if="errors.has('email')" class="is-danger">
      {{ errors.first('email') }}
    </p>
<span class="help is-danger" v-show="emailError">
 {{ emailError }}
 </span>
<span class="help is-danger" v-show="emailError2">
 {{ emailError2 }}
 </span>
   
</div>
 </div>


 <hr id='left-line'>
 
 <div class="field">
 <label for="card_number">Card Number</label>
 <input id="card_number" v-model="card.number" type="text" :class="['is-danger' ? cardNumberError : '', 'input']" placeholder='4242424242424242'>
 <span class="help is-danger" v-show="cardNumberError">
 {{ cardNumberError }}
 </span>
 </div>
<div class="field">
 <label for="cvc">CVC</label>
 <input id="cvc" v-model="card.cvc" type="text" class="input" placeholder='123'>
 <span class="help is-danger" v-show="cardCvcError">
 {{ cardCvcError }}
 </span>
 </div>
<div class="field">
 <label for="exp_month">Expiry Month</label>
 
 <input id="exp_month" v-model="card.exp_month" type="text" :class="['is-danger' ? cardMonthError : '', 'input']" placeholder="MM">
 <span class="help is-danger" v-show="cardMonthError">
 {{ cardMonthError }}
 </span>
 </div>
<div class="field">
 <label for="exp_month">Expiry Year</label>
 <input id="exp_year" v-model="card.exp_year" type="text" :class="['is-danger' ? cardYearError : '', 'input']" placeholder="YY">
 <span class="help is-danger" v-show="cardYearError">
 {{ cardYearError }}
 </span>
 </div>
 
 <div class="help is-danger" v-if="cardCheckError">
 <span>{{ cardCheckErrorMessage }}</span>

 </div> 

 </div>




 <div class='column is-4 padded'>
<div class="divider"></div>
 <label>Special Note</label>
 <textarea class="textarea" placeholder="What would you like the note to say?" v-model='specialNote'></textarea>
 
 <hr>
 
 <div class="field">
 <label>Address</label>
 <input type='text' class='input' v-model='address.street' placeholder='123 Fake St #303'>
 </div>
 <div class="field">
 <label>City</label>
 <input type='text' class='input' v-model='address.city' placeholder='San Francisco'>
 </div>
 <div class="field">
 <label>State</label>
 <input type='text' class='input' v-model='address.state' placeholder='CA'>
 </div>
 <div class="field">
 <label>Zip</label>
 <input type='text' class='input' v-model='address.zip' placeholder='94607'>
 </div>
 </div>
</div>
</div>

 <div class="columns">
<div class='column is-3'>

</div>
<div class='column is-3 has-text-centered'>
<label v-model='roomCost'>
${{ roomCost }} per night<br>
{{ numNights }} nights<br>
X &nbsp; %7 MT state tax &nbsp;
<hr width= "140px" id='center-line'>
${{ totalCost }}</label>
</div>
<div class='column is-3 has-text-centered'>
<br>
<br>
 <button type="submit" class="button is-primary is-large is-center" @click.prevent="validate" :disabled="cardCheckSending">
 <span v-if="cardCheckSending">
 <i class="fa fa-btn fa-spinner fa-spin"></i> 
 Ordering…
 </span>
 <span v-else>
 Pay for Reservation
 </span>
 </button>
 </div>
 
 <div class='column is-3'>

</div>
 </div>
 </div>

</template>
<style>
h2 { text-decoration: underline; }
.textarea:not([rows]) { max-height: 110px; min-height: 110px; }
.container { margin-bottom: 30px; }
.column > img { margin-top: 60px;  margin-right: 60px; margin-left: 60px;}
.button-field { display: flex; justify-content: center; }
#left-line { margin-top:27px; }
#center-line { margin-left: auto;
margin-right: auto; }
.vertical-divider{
	clear: both;
	position: relative;
}

.vertical-divider:after {
	clear: both;
	content: " ";
	display: block;
	height: 0;
	visibility: hidden;
}

.vertical-divider .column:not(:first-child):after, .vertical-divider .columns:not(:first-child):after{
	background: #DDDDDD;
	bottom: 0;
	content: " ";
	margin-left: -10px;
	position: absolute;
	top: 0;
	width: 1px;
}
.hr {
text-align: center;
}
.padded {padding: 25px;}
</style>
<script>
import axios from 'axios';
import HotelDatePicker from 'vue-hotel-datepicker'
import moment from 'moment'
import VeeValidate from 'vee-validate';

const dt = new Date();

export default {
 data(){
  return {

	
      minNights: 2,
      maxNights: 29,
      showCloseButton: true,
	startDate: dt.setDate(dt.getDate() + 3),
	endDate: dt.setDate(dt.getDate() + 365),

	totalCost: 0,
	roomCost: 40.00,
	numNights: 0,

	checkindate: '',
        checkoutdate: '',
	description: '',

   stripeKey: 'pk_test_UBQcB0xVzqqoX0Gho88atD9B',
// fields
	
   name: 'John Doe',
   email: 'email@email.com',
   specialNote: 'Any special requests.',
   address: {
     street: '123 Something Lane',
     city: 'San Francisco',
     state: 'CA',
     zip: '94607'
   },
   card: {
     number: '4242424242424242',
     cvc: '123',
     exp_month: '01',
     exp_year: '19'
   },
// validation
   cardNumberError: null,
   cardCvcError: null,
   cardMonthError: null,
   cardYearError: null,
   cardCheckSending: false,
   cardCheckError: false,
   cardCheckErrorMessage: '',
emailError: null,
stayError: null
  }
 },
 components: {
	HotelDatePicker,
	VeeValidate
 },

 methods: {
setcheckin: function (checkin) {
    this.checkindate= checkin,

this.totalCost= parseFloat(0).toFixed(2);

  },
setcheckout: function (checkout) {
    this.checkoutdate= checkout;

this.a= moment(this.checkoutdate, "MM-DD-YYYY");
this.b= moment(this.checkindate, "MM-DD-YYYY");

this.numNights= this.a.diff(this.b, 'days');

this.totalCost= this.roomCost * this.numNights * 1.07;

this.totalCost= parseFloat(Math.round(this.totalCost * 100) / 100).toFixed(2);

this.description= this.checkindate + this.checkoutdate + this.specialNote;
  },
moment: function () {
    return moment();
  },
   validate(){
     this.clearCardErrors();
     let valid = true;
     if(!this.card.number){ valid = false; this.cardNumberError = "Card Number is Required"; }
     if(!this.card.cvc){ valid = false; this.cardCvcError = "CVC is Required"; }
     if(!this.card.exp_month){ valid = false; this.cardMonthError = "Month is Required"; }
     if(!this.card.exp_year){ valid = false; this.cardYearError = "Year is Required"; }
if(!this.email){ valid = false; this.emailError = "Email is Required"; }


//this.$validator.validateAll().then(() => {
//        alert('Hello, ' + this.email)
//      }).catch(() => {
//        alert(this.errors.all())
//      })


if(!this.checkoutdate){ valid = false; this.stayError = "Dates of stay are Required"; }
     if(valid){
       this.createToken();
     }
   },
   clearCardErrors(){
     this.cardNumberError = null;
     this.cardCvcError = null;
     this.cardMonthError = null;
     this.cardYearError = null;
this.stayError = null;
this.emailError = null;
this.emailError2 = null;
   },
   createToken() {
     this.cardCheckError = false;
     window.Stripe.setPublishableKey(this.stripeKey);
     window.Stripe.createToken(this.card, $.proxy(this.stripeResponseHandler, this));
     this.cardCheckSending = true;
   },
   stripeResponseHandler(status, response) {
     this.cardCheckSending = false;
     if (response.error) {
       this.cardCheckErrorMessage = response.error.message;
       this.cardCheckError = true;
       console.error(response.error);
     } else {
       // token to create a charge on our server 
       var token_from_stripe = response.id;
       var request = {
         name: this.name,
         email: this.email,
         address: this.address,
         card: this.card,
	 amount: this.totalCost,
	 description: this.description,
         token_from_stripe: token_from_stripe
       };
       // Send to our server
       axios.post(`${window.endpoint}/charge`, request)
         .then((res) => {
           var error = res.data.error;
           var charge = res.data.charge;
           if (error){
             console.error(error);
           } else {
             this.$router.push({ path: `order-complete/${charge.id}` })
           }
       }
);
     }
   }
 }
}
</script>