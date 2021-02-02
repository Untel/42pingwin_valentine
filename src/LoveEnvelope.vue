<template>
<div class="wrap">
    <div class="all_container">
        <div class="container">
            <div class="envelope"></div>
            <div ref="flip" class="flip"></div>
            <div ref="letter" class="letter">
                <div contenteditable class="text-content" v-html="message">
                </div>
                <div class="text">
                </div>
            </div>
            <ClippedImage :loading="loading" class="clipped" :url="picture" />
        </div>
        <label class="target" for="target">
            <div>For</div> 
            <input focus id="target" type="text" placeholder="Your crush loggin"
                @change="$emit('onLoginTyped', $event.target.value)"
                @keydown.enter="$emit('onLoginTyped', $event.target.value)"
                @focus="$emit('onLoginTyped', null)"/>
        </label>
    </div>
</div>
</template>
<script>
import ClippedImage from './ClippedImage'

export default {
    props: ['message', 'picture', 'loading'],
    components: { ClippedImage },
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
            },1000);
        },

        close(){
            console.log('close');
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
            }, 1000);
        },
    },
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
    width: auto;
    max-height: 250px;
    overflow-y: auto;
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

.wrap {
    // overflow-x: hidden;
    @include xs {
        transform: scale(.4);
    }
    @include sm {
        transform: scale(.5);
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
// .text{
// 	top: 0.625rem * $m;
// 	left:12px * $m;
// 	height:6px * $m;
// 	width:100px * $m;
// 	background: $txt;
//   }
// .text:before,.text:after{
// 	content:'';
// 	height:100% * $m;
// 	background: $txt;
// 	left:0px;
// 	}
// .text:before{
// 	top:0.625rem * $m;
// 	width: 50px * $m;
// 	}
// .text:after{
// 	top:20px * $m;
// 	width:65px * $m;
//   }
/*Classes to be Added and removed*/
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