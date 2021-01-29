<template>
  <div class="main">
    <!-- <div class="header">
      <img src="/assets/type-logo.png"/>
    </div> -->
    <div class="envelope" @click="toggle">
      <LoveEnvelope ref="envelope"
        :message="`Nathan,\n tu es plus beau qu'un orang outan,<br/> avec toi je voudrais des enfants.<br/> Si tu me prend, ici, maintenant,<br/> je t'en serais reconnaissant ❤️`"
        picture="https://cdn.intra.42.fr/users/nlafarge.jpg" />
    </div>
    <footer class="made_with">
      Made with ❤️ by <a href="https://github.com/untel">adda-sil</a>
    </footer>
    <LoveBackground />
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
  async mounted() {
    const login = await axios.get('http://localhost:8888/valentine?login=adda-sil');
    console.log(login)
    // setTimeout(() => this.toggle(), 1000);
  },
  methods: {
    toggle() {
      const ref = this.$refs.envelope;
      if (ref.opened) ref.close();
      else ref.open();
    }
  }
}
</script>

<style lang="scss">
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
  background-color: var(--color2);
}
html, body, #app, .main {
  height: 100%;
  margin: 0;
  padding: 0;
}

.envelope {
  height: 95%;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.made_with {
  color: var(--color3);
  font-weight: 400;
  width: 100%;
  text-align: center;
  z-index: 40;
}

body {
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Oxygen-Sans,Ubuntu,Cantarell,"Helvetica Neue",sans-serif;
}
a {
  text-decoration: none;
  color: var(--color3) }
</style>
