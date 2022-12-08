<template>
  <div class="wrap">
    <div class="all_container">
      <div class="container">
        <div class="envelope"></div>
        <div ref="flip" class="flip"></div>
        <div ref="letter" class="letter">
          <textarea :disabled="loading" contenteditable class="text-content" v-model="val">
          </textarea>
          
        </div>
        <ClippedImage :loading="loading" class="clipped" :url="picture" />
      </div>
      <label class="target" for="target">
        <div>For</div> 
        <input id="target" type="text" v-model="login" placeholder="Your crush login"
        @blur="shouldEmitLogin"
        @keyup.enter="$event.target.blur()"/>
      </label>
      <div v-if="opened" class="sendLove">
        <button :disabled="loading" @click="$emit('send', val)">Send</button>
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-env jquery */

import ClippedImage from './ClippedImage'

export default {
  props: ['message', 'picture', 'loading'],
  components: { ClippedImage },
  data: () => ({ val: '', opened: false, login: '' }),
  methods: {
    open(){
      if (this.opened)
      return;
      var flip = this.$refs.flip;
      var letter = this.$refs.letter;
      flip.classList.add('open');
      flip.classList.remove('close');
      setTimeout(() => {
        letter.style.zIndex = '7';
      }, 500);
      setTimeout(() => {
        letter.classList.add('letterOpen');
        letter.classList.remove('letterClose');
        this.opened = true;
      },800);
    },
    
    close() {
      if (!this.opened)
      return;
      var flip = this.$refs.flip;
      var letter = this.$refs.letter;
      letter.classList.remove('letterOpen');
      letter.classList.add('letterClose');
      setTimeout(() => {
        flip.classList.remove('open');
        flip.classList.add('close');
        letter.style.zIndex = '5';
        console.log('Should close', this.opened)
        this.opened = false;
      }, 800);
    },
    shouldEmitLogin() {
      if (this.login.length <= 8)
      this.$emit('onLoginTyped', this.login);
    }
  },
  watch: {
    message(value) {
      this.val = value;
    }
  }
}
</script>
<style lang="scss" scoped>
@import 'breakpoints';

.target {
  position: absolute;
  right: 0;
  bottom: 50%;
  display: flex;
  flex-direction: row;
  align-content: center;
  align-items: center;
  z-index: 200;
  font-family: 'love';
  font-size: 2.5rem;
  color: white;
  max-width: 30%;
  div {
    margin-right: 20px;
  }
  input {
    font-family: 'love';
    font-size: 2.5rem;
    color: white;
    display: flex;
    min-width: 0;
    justify-content: center;
    align-items: center;
    flex-grow: 1;
    background-color: transparent;
    border: none;
    border: 2px dashed var(--color4);
    padding: 0.625rem;
    border-radius: 1.875rem;
    
    &:focus {
      outline: none;
    }
    
    &::placeholder {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
      font-size: 20px;
      font-style: italic;
      color: rgba(70, 70, 70, 0.5);
    }
  }
}

.text-content {
  padding: 1.875rem;
  font-family: 'love';
  font-size: 2.5rem;
  color: #272778;
  width: calc(100% - 1.875rem * 2);
  border: none;
  height: 14rem;
  max-height: 250px;
  overflow-y: auto;
  background-color: transparent;
  cursor: text;
  
  &:focus {
    outline: none;
  }
}

.clipped {
  z-index: 20;
  margin-left: 1.25rem;
  margin-top: 3.125rem;
  transform: rotate(-30deg)
}

$s: solid;
$a: absolute;
$m: 6;
$tb:2.875rem * $m;
$lr:4.375rem * $m;
$br:0.375rem;
//color variables
$left:#fa565a;
$right:#ed4c50;
$bottom:#fa565a;
$txt: #b0b0b0;
$top: #c94548;

.all_container {
  position: relative;
  height: 5.75rem * $m;
  width: 8.5rem * $m;
}

.sendLove {
  position: absolute;
  top: 100px;
  right: 0;
  left: 0;
  z-index: 15;
  display: flex;
  justify-content: center;
  button {
    cursor: pointer;
    font-family: 'love';
    background-color: white;
    font-size: 2.5rem;
    background-color: transparent;
    border: 2px dashed var(--color4);
    padding: 20px;
    border-radius: 40px;
    color: var(--color1);
    
    &:hover {
      background-color: var(--color2);
      color: white;
      border: none;
    }
  }
}

.wrap {
  // overflow-x: hidden;
  @include xxs {
    transform: scale(.35);
  }
  @include xs {
    transform: scale(.6);
  }
  @include sm {
    transform: scale(.7);
  }
  @include md {
    transform: scale(.8);
  }
}

.container,.envelope,.flip,.letter {
  left: 0;
}

.container,.envelope,.flip,.letter,.text,.text:before,.text:after{
  position: $a;
}
.envelope{
  border-top: $tb $s transparent;
  border-left: $lr $s $left;
  border-bottom: $tb $s $bottom;
  border-right: $lr $s $right;
  border-radius: $br;
  z-index:9;
}
.flip{
  top:0;
  left:0;
  border-top: $tb $s $top;
  border-left: $lr $s transparent;
  border-bottom: $tb $s transparent;
  border-right: $lr $s transparent;
  border-radius: $br;
  z-index:6;
}
.letter{
  top:0px * $m;
  left:0.125rem * $m;
  height: 5.75rem * $m;
  width: 8.5rem * $m;
  background: #ddd;
  border-radius: $br;
  z-index:1;
}

.open{
  animation: flipOpen .4s ease-in forwards ;
}

.close{
  animation: flipClose .4s ease-in forwards;
}

.letterOpen{
  height: (5.75rem * $m) * 1;
  top:(-2.5rem * $m) * 1;
  transition: .3s ease-in;
}

.letterClose{
  top: 0px * $m * 1;
  transition: .3s ease-in;
}
//animations
@keyframes flipOpen{
  0%{
    transform: rotateX(0deg);
    transform-origin:center top;
  }
  100%{
    transform:rotateX(180deg);
    transform-origin:center top;
  }
}
@keyframes flipClose{
  0%{
    transform: rotateX(180deg);
    transform-origin:center top;
  }
  100%{
    transform:rotateX(0deg);
    transform-origin:center top;
  }
}

</style>