<template>

<div id="parent_translate">

<div id="input">

<div id="imageholder">

<select selected="cow" v-model="FromLanguage" @change="changeFromLanguage">

<option v-for="(lang, code) in AvailableLanguages" :value="code"> {{ lang }} </option>
  
</select>

<img @click="translateSpeak" src="../assets/speaker.png">

</div>

<textarea placeholder="Type in something here" v-model="Text"></textarea>

<button @click="submit" id="submit">SUBMIT</button>
</div>

<div id="output">


<div id="imageholder">

<select v-model="ToLanguage" @change="changeToLanguage">

  <option v-for="(lang, code) in AvailableLanguages" :value="code"> {{ lang }} </option>

</select>

<img @click="responseSpeak" src="../assets/speaker.png">

</div>

<textarea placeholder="Your output goes here" disabled v-model="ResponseText"> </textarea>

</div>

</div>

</template>

<script>
export default {
  name: 'translate',
 
  props: {

   AvailableLanguages : {
     type : Object
   },

   ResponseText : {
     type : String
   },

   CurrentLanguage : {
     type : String
   },

   TranslateLanguage : {
     type : String
   }
 },

methods: {

  changeFromLanguage:function( value ) {
    let LanguageCode = value.target.value;
    this.$emit('updateFromLanguage', LanguageCode)
  },

  changeToLanguage:function( value ) {
    let LanguageCode = value.target.value;
    this.$emit('updateToLanguage', LanguageCode)
  },

  submit:function( text ){
    let Word = this.Text
    this.$emit('translate', Word)
  },

  translateSpeak:function(){
    responsiveVoice.speak(this.Text);
  },

  responseSpeak:function(){
    responsiveVoice.speak(this.ResponseText);
  }

},

  data () {
    return {
      Text: '',
      FromLanguage : "English",
      ToLanguage : "Russia"
    }
  }
}
</script>

<style scoped>

#parent_translate {
  width: 100%;
  height: 100%;
  display: flex;
  flex-wrap: nowrap;
  flex-direction: row;
}

select {
  padding: 0.2em 0em;
  margin: 0;
  font-family: 'Indie Flower', cursive;
}

#submit {
background: #4ed34c;

padding: 0.7em;
border: none;
border-radius: 1px;
cursor: pointer;
color: black;
}

#input, #output {
  background-color: #f1f1f1;
  width: inherit;
  height: inherit;
  text-align: center;
  font-size: 30px;
  border-radius: 60px;
}

#input {

  margin: 0px 2px 0px 0px;
}

#output {
    margin: 0px 0px 0px 2px;  
}

textarea {
  width: inherit;
  height: 60%;
  margin: 0px;
  padding: 45px 20px;
  box-sizing: border-box;
  outline: none;
  border: none;
  text-align: center;
  font-size: 25px;
  font-family: 'Indie Flower', cursive;
  font-weight: bold;
}

#input > div, #output > div {
  width: inherit;
  height: 20%;
  margin: 0px;
  padding: 0px;
}

#input > div > img, #output > div > img {
 padding: 10px 0px 0px 0px;
 width: 12%;
 height: 60%;
 cursor: pointer;
}

</style>
