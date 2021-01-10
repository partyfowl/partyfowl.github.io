<template>
  <div>
    <header>
      <p @click="home()" style="text-align: left; padding-left: 10px">Home</p>
      <h1 id="page-title" v-if="title">{{ title }}</h1>
      <p>
        {{ date }}
      </p>
      <p id="quote">{{ quote }}</p>
    </header>
    <div id="albums">
      <figure class="album" v-for="image in images" :key="image.img">
        <div @click="route(image)" class="img-container" :style="{ backgroundImage: 'url(\'' + image.img + '\')' }"></div>
        <figcaption v-if="image.caption && image.date">{{ image.caption }} ({{ image.date }})</figcaption>
        <figcaption v-else-if="image.caption">{{ image.caption }}</figcaption>
      </figure>
    </div>

    <div :style="modalDisplay">
      <button @click="closeModal()" id="close-modal">Close</button>
      <img :src="modalImg" id="modal-img">
    </div>

  </div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "MainPage",
    data() {
      return {
        title: null,
        date: null,
        quote: null,
        images: null,
        modalDisplay: "display: none",
        modalImg: null,
        loading: true,
        errored: false
      }
    },
    mounted() {
      this.home();
    },
    methods: {
      home: function () {
        this.loadImages('https://partyfowl.github.io/mock-api-response.json')
      },
      route: function(image){
        if (image.album){
          this.loadImages(image.album)
        }
        else {
          this.modalDisplay = "display: block; position: absolute; z-index: 1; left: 10px; top: 10px; right: 10px",
          this.modalImg = image.img
        }
      },
      closeModal: function() {
        this.modalDisplay = "display: none";
      },
      loadImages: function (uri) {
        axios.get(uri)
          .then(response => {
            let data = response.data
            this.title = data.title
            this.date = data.date
            this.quote = data.quote
            this.images = data.images
          })
          .catch(error => {
            console.log(error)
            this.errored = true
          })
          .finally(() => (this.loading = false))
      }
    }
  }
</script>

<style>
  #modal-img {
     max-width: 100%;
     height: auto;
  }

  #close-modal {
    position: absolute;
    left: 100%;
    transform: translate(-100%);
  }

  @media screen and (max-width: 1279px) {
    body {
      font-size: 150%;
    }
  }


  #quote {
    font-style: italic;
  }

  body {
    background-color: #202020;
    color: white;
    margin: auto;
    padding: 0;
    text-align: center;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }

  p {
    font-size: 1em;
  }

  h1 {
    font-size: 2.5em;
  }

  figure {
    font-size: 1em;
  }

  #albums {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    width: 100%;
    flex-wrap: wrap;
  }

  figcaption {
    padding-bottom: 8px;
  }

  .album {
    margin: 0px;
  }

  @media only screen and (max-width: 540px) {
    .album {
      flex-basis: 100%;
    }
  }

  @media only screen and (min-width: 541px) and (max-width: 1080px) {
    .album {
      flex-basis: 50%;
    }
  }

  @media only screen and (min-width: 1081px) and (max-width: 1620px) {
    .album {
      flex-basis: 33.3333333333%;
    }
  }

  @media only screen and (min-width: 1621px) and (max-width: 2160px) {
    .album {
      flex-basis: 25%;
    }
  }

  @media only screen and (min-width: 2161px) {
    .album {
      flex-basis: 20%;
    }
  }


  .img-container {
    position: relative;
    /* padding-bottom: 56.25%; */
    padding-bottom: 100%;
    background-size: cover;
    background-position: center;
  }
</style>