<template>
  <div class="main">
    <LoveBackground v-if="showBackground" />
    <div class="header">
      <img class="mascotte" src="@/assets/professeur.png" alt="">
      <div class="speech-bubble">
        <div v-if="!message">
          Salut jeune pingwin !<br/>
          Ton BDE préféré
          <img style="vertical-align: middle" src="@/assets/logo-ping.png" height="10"/>
          te permet de laisser un message à ton admirateur secret !<br/>
          Écris son login sur l'enveloppe pour écrire ta lettre d'amour 😍<br/>
          Et pense a revenir ici le 14 février pour récupérer les tiens.
        </div>
        <div v-if="message" v-html="message">

        </div>
      </div>
    </div>
    <!-- <div class="header">
      <img src="/assets/type-logo.png"/>
    </div> -->
    <div class="LoveEnvelope">
        <LoveEnvelope
          ref="envelope"
          :message="initialMessage"
          :picture="picture"
          :loading="loading"
          @onLoginTyped="searchLogin"
          @send="sendLove"/>
    </div>
    <div class="made_with">
      Made with ❤️ by <a target="_blank" href="https://github.com/untel">adda-sil</a>
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
    return { showBackground: false, picture: '', loading: false, message: '', loggin: '', initialMessage: '' };
  },
  async mounted() {
    // const login = await axios.get('http://localhost:8888/valentine?login=adda-sil');
    // console.log(login)
    // // setTimeout(() => this.toggle(), 1000);
  },
  methods: {
    async sendLove(form) {
      if (!this.loggin || !form || !form.length) {
        this.message = `Oops<br/>Il semble que qu'il me manque des informations pour envoyer ton message.`
        return;
      }
      this.showBackground = true;
      this.loading = true;
      const res = await axios.post(`http://localhost:8888/api?form`, { loggin: this.loggin, message: form });
      console.log('Ret is', res);
      if (res.status === 200 && res.data.success) {
        this.message = `Ton message a bien été enregistré 😍<br/> Tu peux en envoyer à un autre étudiant 🤫<br/>`
        this.showBg(5);
        this.loggin = null;
        this.picture = null;
        this.$refs.envelope.close();
      } else {
        this.message = `Oops<br/>Une erreur est survenue lors de l'envoi du message 😱 Reviens plus tard !<br/>`
        this.showBackground = false;
      }
      this.loading = false;
    },
    toggle() {
      const ref = this.$refs.envelope;
      if (ref.opened) ref.close();
      else ref.open();
    },
    showBg(time) {
      this.showBackground = true;
      setTimeout(() => this.showBackground, (time || 3) * 1000)
    },
    async searchLogin (log) {
      this.$refs.envelope.close();
      this.loading = true;
      this.picture = '';
      this.loggin = null;
      this.showBackground = true;
      if (log) {
        try {
          const req = await axios.get(`http://localhost:8888/api?login=${log}`);
          this.loading = false;
          if (req.data && !req.data.error) {
            this.loggin = log;
            this.picture = req.data.image_url;
            this.message = `Tu peux maintenant éclarer ta flamme à ${req.data.first_name}<br/> en écrivant dans la lettre 😏`;
            this.initialMessage = `${req.data.first_name}, !`
            this.$refs.envelope.open();
            this.showBackground = false;
          } else {
            this.message = `Oops</br>Je n'ai trouvé aucun étudiant 42 avec ce loggin !`;
            this.loggin = null;
            this.$refs.envelope.close();
            this.showBackground = false;
          }
        } catch (e) {
          this.message = `Oops</br>Une erreur est survenue 😭`
          this.loading = false;
          this.loggin = null;
          this.$refs.envelope.close();
          this.showBackground = false;
          console.log(e)
        }
        setTimeout(() => this.message = null, 6000);
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
    display: none;
    @include sm {
      display: block;
    }
    height: 145px;
    width: 200px;
    max-height: 145px;
  }
}
html, body, #app, .main {
  height: -webkit-fill-available;
  margin: 0;
  padding: 0;
}

.LoveEnvelope {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding-top: 85px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

}

.made_with {
  color: white;
  position: absolute;
  bottom: 15px;
  font-weight: 400;
  width: 100%;
  text-align: center;
  z-index: 40;
  font-size: 16px;
  opacity: .8;
  a {
    font-size: 18px;
    font-weight: 600;
    color: var(--color3);
  }
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
