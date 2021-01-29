<template>
<div class="all_container">
    <div class="container">
        <div class="envelope"></div>
        <div ref="flip" class="flip">
            
        </div>
        <div ref="letter" class="letter">
            <div class="text">
                <div contenteditable class="text-content" v-html="message">
                </div>
            </div>
        </div>
        <ClippedImage class="clipped" :url="picture" />
    </div>

</div>
</template>
<script>
import ClippedImage from './ClippedImage'

export default {
    props: ['message', 'picture'],
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
            }, 300);
        },
    },
}
</script>
<style lang="scss" scoped>

.text-content {
    padding: 30px;
    font-family: 'love';
    font-size: 40px;
    color: #272778;
}

.clipped {
    z-index: 20;
    margin-left: 20px;
    margin-top: 50px;
    transform: rotate(-30deg)
}

$s: solid;
$a: absolute;
$m: 6;
$tb:46px * $m;
$lr:70px * $m;
$br:6px;
//color variables
$left:#fa565a;
$right:#ed4c50;
$bottom:#fa565a;
$txt: #b0b0b0;
$top: #c94548;

.all_container {
    position: relative;
    height: 92px * $m;
	width: 136px * $m;
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
	left:2px * $m;
	height: 92px * $m;
	width: 136px * $m;
	background: #ddd;
	border-radius: $br;
	z-index:1;
	}
// .text{
// 	top: 10px * $m;
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
// 	top:10px * $m;
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
	height: (92px * $m) * 1;
	top:(-40px * $m) * 1;
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