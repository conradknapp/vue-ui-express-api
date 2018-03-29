<template lang="pug">
v-app
  v-layout(row wrap)
    div(class="btn-container")
      v-btn(class="text-xs-center" color="info"
               :loading="loading"
               @click="getQuote"
               :disabled="loading") Get Inspired
        span(slot="loader" class="custom-loader")
          v-icon(light) cached
      transition(name="fade")
        v-layout(row wrap class="quote-container" v-if="reveal")
          v-flex(xs12 sm6 offset-sm3)
            h2 {{ quote }}
            p {{ author }}
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return { 
      url: 'https://rest-philosophy-bghtlcwdkt.now.sh/quotes/random',
      quote: '',
      author: '',
      loading: false,
      reveal: false
    }
  },
  created() {
    this.getQuote()
  },  
  methods: {
    getQuote() {
      this.reveal = false;
      this.loading = true;
      axios.get(this.url)
        .then(({ data }) => {
          this.loading = false;
          this.reveal = true;
          let { quotes } = data;
          this.quote = quotes[0].text;
          this.author = quotes[0].author;
        })
        .catch(err => {
          this.loading = false;
          console.warn(err);
        });
    } 
  }
}
</script>

<style lang="sass">
#app
  background-image: linear-gradient(#B5CBED 0%,#FE8A75 100%)
  padding-top: 50px
  color: #fff
  font-weight: 100
 
.btn-container
  display: flex
  margin: 0 auto
  flex-direction: column
  justify-content: center
  align-items: center

.quote-container
  margin-top: 50px
  width: 80vw
  
h2
  font-size: 2rem
  &:hover
    opacity: 0.9
  
.fade-enter
  opacity: 0
  
.fade-enter-active
  transition: opacity 1s

.fade-leave-active
  transition: opacity 1s
  opacity: 0
  
button
  position: absolute
  z-index: 2
  &:hover
    opacity: 0.94
    cursor: pointer
  
p 
  font-size: 1.5rem

.custom-loader 
  animation: loader 1s infinite
  display: flex

@-moz-keyframes loader
  from
    transform: rotate(0)
  to
    transform: rotate(360deg)

@-webkit-keyframes loader
  from
    transform: rotate(0)
  to
    transform: rotate(360deg)
      
@-o-keyframes loader
  from
    transform: rotate(0)
  to
    transform: rotate(360deg)
      
@keyframes loader
  from
    transform: rotate(0)
  to 
    transform: rotate(360deg)
</style>