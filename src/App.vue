<template>
  <div class="main">
    <LoveBackground v-if="showBackground" />
    <div class="header">
      <img class="mascotte" src="@/assets/professeur.png" alt="">
      <div class="speech-bubble">
        <div v-if="!message && !isDay">
          Heyyyy dear pingwin 🐧 !<br/>
          The <i>42 | Paris</i> Social Club
          <img style="vertical-align: middle" src="@/assets/logo-ping.png" height="10"/>
          allows you to send an anonymous message to your secret admirer !<br/>
          Put his/her login on the envelop to write your love letter 😍<br/>
          And don't forget to come back on this website on the 14th of Feburary to check out the letters that got sent to you<br/>
          Icy kisses 🧊💋 my pingwin 
        </div>
        <div v-if="message" v-html="message">

        </div>
      </div>
    </div>
    <div class="LoveEnvelope">
        <LoveEnvelope
          :isDay="isDay"
          ref="envelope"
          :message="initialMessage"
          :picture="picture"
          :loading="loading"
          @onLoginTyped="searchLogin"
          @send="sendLove"/>
    </div>
    <div class="made_with">
      Made with ❤️ &nbsp;by <a target="_blank" href="https://github.com/untel">adda-sil</a>
    </div>
  </div>
</template>

<script>
/* eslint-env jquery */
import axios from 'axios';
import LoveBackground from './LoveBackground'
import LoveEnvelope from './LoveEnvelope'
axios.defaults.baseURL = '/';
export default {
  name: 'App',
  components: {
    LoveBackground,
    LoveEnvelope,
  },
  data() {
    return {
      showBackground: false,
      picture: '',
      loading: false,
      message: '',
      loggin: '',
      initialMessage: '',
      isDay: false,
      endpoint: 'http://localhost:8888/',
      // endpoint: '',
    };
  },
  methods: {
    async sendLove(form) {
      if (!this.loggin || !form || !form.length || this.loading) {
        this.message = `Oops<br/>Some informations are missing.`
        return;
      }
      this.showBackground = true;
      this.loading = true;
      const res = await this.sendDataForm(form, this.loggin);
      this.loading = false;
      if (res.status === 200 && res.data.success) {
        this.message = `Your message has been saved 😍<br/> Buuut, you can send an other one to someone else 🤫<br/>`
        this.showBg(5);
        this.loggin = null;
        this.picture = null;
        this.$refs.envelope.close();
      } else {
        this.message = `Oops<br/>An error occured 😱 Come back later !<br/>`
        this.showBackground = false;
      }
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
    async fetchLogin(log) {
      if (process.env === 'production') {
        return await axios.get(`${this.endpoint}/api?login=${log}`)
      }
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ data: {
            first_name: 'Adrien',
            image_url: 'https://cdn.intra.42.fr/users/bbe3a4ac809728a21b15436c6d7aa474/adda-sil.jpg',
          } });
        }, 1000);
      })
    },

    async sendDataForm(form, log) {
      if (process.env === 'production') {
        return await axios.post(`${this.endpoint}/api`, { form, log })
      }
      
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ status: 200, data: { success: true } });
        }, 1000);
      })
    },

    async searchLogin (log) {
      if (log) {
        this.loading = true;
        this.picture = '';
        this.loggin = null;
        this.showBackground = true;
        try {
          const req = await this.fetchLogin(log);
          this.loading = false;
          if (req.data && !req.data.error) {
            this.loggin = log;
            this.picture = req.data.image_url;
            this.message = `You can now secretly declare your love to ${req.data.first_name}<br/> by writing in the letter 😏`;            this.initialMessage = `${req.data.first_name}, ...`
            this.$refs.envelope.open();
            this.showBackground = false;
          } else {
            this.message = `Oops</br>I found no 42 student with this login 🧐 !`;
            this.loggin = null;
            this.$refs.envelope.close();
            this.showBackground = false;
          }
        } catch (e) {
          this.message = `Oops</br>An error occured 😭`
          this.loading = false;
          this.loggin = null;
          this.$refs.envelope.close();
          this.showBackground = false;
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

  @media (max-width: 320px) {
    overflow: hidden;
  }
}

.LoveEnvelope {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  @media (max-width: 321px) {
    top: 50px;
    bottom: -150px;
  }

}

.made_with {
  color: white;
  position: absolute;
  bottom: 15px;
  right: 15px;
  font-weight: 400;
  text-align: center;
  z-index: 40;
  font-size: 12px;
  opacity: .5;
  a {
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
