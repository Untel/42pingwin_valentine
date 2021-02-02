<template>
  <div class="main">
    <div class="header">
      <img class="mascotte" src="@/assets/professeur.png" alt="">
      <div class="speech-bubble">
        <div v-if="!message">
          Salut jeune pinguin !<br/>
          Ton BDE préféré
          <img style="vertical-align: middle" src="@/assets/logo-ping.png" height="10"/>
          te permet de laisser un message à ton admirateur secret !<br/>
          Écrit son login sur l'enveloppe afin de pouvoir écrire ton message<br/>
          Et pense a revenir ici le 14 février pour récupérer les tiens.
        </div>
        <div v-if="message" v-html="message">

        </div>
      </div>
    </div>
    <!-- <div class="header">
      <img src="/assets/type-logo.png"/>
    </div> -->
    <!-- <LoveBackground /> -->
    <div class="LoveEnvelope">
        <LoveEnvelope
          ref="envelope"
          :message="'Declare ta flamme ici'"
          :picture="picture"
          :loading="loading"
          @onLoginTyped="searchLogin"/>
    </div>
    <div class="made_with">
      Made with ❤️ by <a href="https://github.com/untel">adda-sil</a>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import LoveBackground from './LoveBackground'
import LoveCheckbox from './LoveCheckbox'
import LoveEnvelope from './LoveEnvelope'
import ClippedImage from './ClippedImage'
export default {
  name: 'App',
  components: {
    LoveBackground,
    LoveCheckbox,
    LoveEnvelope,
    ClippedImage,
  },
  data() {
    return { picture: '', loading: false, message: '' };
  },
  async mounted() {
    // const login = await axios.get('http://localhost:8888/valentine?login=adda-sil');
    // console.log(login)
    // // setTimeout(() => this.toggle(), 1000);
  },
  methods: {
    toggle() {
      const ref = this.$refs.envelope;
      if (ref.opened) ref.close();
      else ref.open();
    },
    async searchLogin (log) {
      this.$refs.envelope.close();
      this.loading = true;
      this.picture = '';
      if (log) {
        try {
          const req = await axios.get(`http://localhost:8888/api?login=${log}`);
          this.loading = false;
          console.log('req', req.data)
          if (req.data) {
            this.$refs.envelope.open();
            this.picture = req.data.image_url;
          } else {
            this.message = `Oops</br>Je n'ai trouvé aucun étudiant 42 avec ce loggin !`
          }
        } catch (e) {
          console.log(e)
        }
      }
    },
  }
}
</script>

<style lang="scss">
@import 'breakpoints';
@font-face {
  font-family: 'love';
  src: url('~@/assets/fonts/font.ttf'); /* IE9 Compat Modes */
}
#app {
  --color1: #c8005f;
  --color2: #ff4770;
  --color3: #ffbbca;
  --color4: #ff8db2;
  --color5: #86001e;
}


.main {
  background-color: var(--color1);
}
.header {
  padding: 30px;
  display: flex;

  .mascotte {
    height: 145px;
    width: 200px;
    max-height: 145px;
  }
}
html, body, #app, .main {
  height: 100%;
  margin: 0;
  padding: 0;
}

.LoveEnvelope {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding-top: 85px;
  cursor: pointer;

}

.made_with {
  color: var(--color3);
  font-weight: 400;
  width: 100%;
  text-align: center;
  z-index: 40;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
a {
  text-decoration: none;
  color: var(--color3) }

  .speech-bubble {
    padding: 20px;
    position: relative;
    background: white;
    border-radius: 30px;
    height: max-content;
    max-height: 100px;
    overflow-y: auto;
  }

.speech-bubble:after {
	content: '';
	position: absolute;
	left: 0;
	top: 50%;
	width: 0;
	height: 0;
	border: 20px solid transparent;
	border-right-color: white;
	border-left: 0;
	border-bottom: 0;
	margin-top: -10px;
	margin-left: -20px;
}
</style>
