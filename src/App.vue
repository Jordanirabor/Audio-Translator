<template>

<div id="app">

<h2>VUE AUDIO TRANSLATOR </h2>

<div id="translate">

<translate v-bind:AvailableLanguages="AvailableLanguages" v-bind:TranslateText="TranslateText" v-bind:ResponseText="ResponseText"  @updateFromLanguage="updateFromLanguage($event)" @updateToLanguage="updateToLanguage($event)" @translate="handleTranslation($event)">
</translate>

</div>

</div>

</template>

<script>

import translate from './components/translate.vue'

export default {
  name: 'app',

  components: {
    translate : translate
  },

  data () {
    return {
      TranslateText : '',
      ResponseText : '',
      CurrentLanguage : 'en',
      TranslateLanguage : 'fr',
      AvailableLanguages : {}
    }
  },
  
 created(){

  var self = this;

 this.axios.get('https://translate.yandex.net/api/v1.5/tr.json/getLangs?ui=en&key=trnsl.1.1.20171231T152156Z.5ff5563907fb4cbb.133f77517eab83da0b5986fbb59d989b7c2cd294')
  .then(function (response) {
    self.AvailableLanguages = response.data.langs;
  })
  .catch(function (error) {
    console.log(error);
  })

  },

  methods: {

  updateFromLanguage:function( value ){
    this.CurrentLanguage = value;
    this.translate();
  },

  updateToLanguage:function( value ){
    this.TranslateLanguage = value;
    this.translate();
  },

  handleTranslation:function( value ){
    this.TranslateText = value;
    this.translate();
  },

translate:function(){

  var self = this;

 this.axios.get('https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20171231T152156Z.5ff5563907fb4cbb.133f77517eab83da0b5986fbb59d989b7c2cd294&text='+self.TranslateText+'&lang='+self.CurrentLanguage+'-'+self.TranslateLanguage+'&format=plain')
  .then(function (response) {

  self.validateResponse(response);
  })
  .catch(function (error) {
    console.log(error);
  })

  },

  validateResponse:function(response){

        if (response.status === 422) {

            console.log('Could Not Translate Text Please Try Again Later');

        } else if (response.status === 413) {

            console.log('Please Enter A Shorter Text to translate');

        } else if (response.status===200) { 

           this.ResponseText = response.data.text[0]

        } else {
          
          console.log('Cannot translate at this moment please try again later');
        }
    }

  }
}
</script>

<style scoped>

#translate {
  height: 45vh;
  max-width: 90%;
  margin: 2.5vh auto;
}


h2 {
  text-align: center;
  font-size: 1.6rem;
  color: black;
  font-weight: bold;
  font-family: 'Indie Flower', cursive;
}

</style>
