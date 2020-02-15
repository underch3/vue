<template>
  <div class="gallery">
    <!-- <button @click="test">test</button> -->
    <ul class="list" v-if="images.length > 1">
      <li v-for="image in images" :key="image.id">
        <img
          class="image"
          :src="image.url"
          :alt="`Meme named: ${image.name}`"
          @click="
            modalShow = !modalShow;
            imageInModal = image.url;
          "
        /> 
      </li>
    </ul>
    <b-modal size="lg" centered id="modal-1" v-model="modalShow" title="Stylo Your Meme of Destiny" hide-footer="true" >
      <Modal :imageInModal="imageInModal"/>
    </b-modal>
  </div>
</template>

<script>
import Modal from "./Modal";
export default {
  components: {
    Modal
  },
  name: "Gallery",
  props: {},
  data() {
    return {
      images: [],
      modalShow: false,
      imageInModal: ""
    };
  },
  methods: {
  },
  created() {
    fetch("https://api.imgflip.com/get_memes")
      .then(response => response.json())
      .then(response => {
        this.images = response.data.memes; //.filter(item => item.width === item.height) in case you want to filter
      });
  }
};
</script>

<style scoped>
.list {
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  justify-content: center;
}
.image {
    background-color: white;
    border-radius: 4px;
    margin: 3px;
    width: 300px;
    height: 300px;
    display: inline-block; /* makes it fit in like an <img> */
    background-size: contain; /* or contain */
    background-repeat: no-repeat;
    object-fit: cover;
}
</style>
