<template>
    <div class="container">
      <transition name="slide-fade" mode="out-in">
        <div class="content"  :key="randomOne.plain_text">
          <h2>{{ randomOne.plain_text }}</h2>
          <h1>{{ randomOne.emoji }}</h1>
        </div>
      </transition>

      <div class="blob">
      <svg
        width="800"
        height="400"
        viewBox="0 0 600 600"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g transform="translate(300,300)">
          <path d="M165.6,-137.9C203.4,-84.6,215.3,-16.4,200.1,43.9C185,104.2,142.8,156.6,85,189.4C27.2,222.2,-46.3,235.5,-98.5,208.7C-150.7,181.9,-181.6,115.2,-198.9,44.1C-216.2,-27.1,-219.9,-102.7,-184.5,-155.5C-149.1,-208.2,-74.5,-238.1,-5.3,-233.8C63.8,-229.6,127.7,-191.2,165.6,-137.9Z" fill="#216583" />
        </g>
      </svg></div>

      <span class="another" v-on:click="anotherOne">Another one !</span>

      <br/>

      <div class="links">
        <strong>Emoji Speaking</strong><br/>
        made by <a href="https://github.com/laurentfite" target="_blank">Laurent Fite</a>
      </div>

    </div>
</template>

<script>
const sanityClient = require('@sanity/client')
const client = sanityClient({
  projectId: '9o7gyodj',
  dataset: 'production',
  token: '', // or leave blank to be anonymous user
  useCdn: false // `false` if you want to ensure fresh data
})

export default {
  metaInfo: {
    title: 'Emoji Speaking'
  },
  data () {
    return {
      items: null,
      message: 'test',
      randomOne: {plain_text: '', emoji: ''}
    }
  },
  methods: {
    anotherOne: function () {
      this.randomOne = this.items[Math.floor(Math.random()*this.items.length)]
    }
  },
  mounted () {
    const query = '*[_type == "post"] {plain_text, emoji}'
    const params = {}

    client.fetch(query, params).then(items => {
      this.items = items
      this.randomOne = this.items[Math.floor(Math.random()*this.items.length)]
    })
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Josefin+Sans&display=swap');

html, body {
  height: 100%; width: 100%;
  background: #293462;
  color: #fff1c1;
  font-family: "Josefin Sans", sans-serif;
  font-size: 150%;

  display: flex;
  justify-content: center;
}
.container {
  position: relative;
  display: flex;
  flex-direction: column;
  align-self: center;

  text-align: center;
  padding: 1em;
}
h2 {
  background-image: linear-gradient(transparent,transparent 20%,#f76262 0,#f76262 100%,transparent 0);
  border-radius: 3px;
  line-height: 0.9;
}
.another {
  font-size:80%;
  z-index: 2;
}
.another:hover {
  cursor: pointer;
}

.links {
  font-size: 40%;
  text-align: center;
  z-index: 3;
}
a {
  color: #f76262;
}

.content {
  z-index: 2;
  transition: width 2s, height 2s, background-color 2s, transform 2s;
}

.blob {
  position: absolute;
  left: 0; top: 0;
  display: flex;
  fill: #216583;
  width: 100%;
  z-index: 1;
  animation: move 10s ease-in-out infinite;
  transform-origin: 50% 50%;
}
@keyframes move {
  0%   { transform: scale(1.5)   translate(10px, -10px); }
  40%  { transform: scale(1.3, 1) translate(5px, -5px) rotate(160deg); }
  80% { transform: scale(1.4)   translate(10px, 20px); }
  100% { transform: scale(1.5)   translate(10px, -10px); }
}
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
</style>
