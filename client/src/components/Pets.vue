<template>
  <div class="wrapper">
    <h1 class="uppercase">unordinary pets</h1>
    <div class="container-flex" ref="container">
      <div class="selected_pet" :class="{columnFlex: readmore}">
        <div class="image">
          <img :src="selectedPet.image_url_hd" alt="pet image">
        </div>
        <div class="content">
          <h2 class="title uppercase">{{selectedPet.name}}</h2>
          <p>
            {{selectedPet.content.highlight}}
            <template v-if="readmore">
              <span>{{selectedPet.content.description}}</span>
              <h4 class="uppercase">Carring out pet</h4>
          <p>{{selectedPet.content.carring_for_pet}}</p>
          <h4 class="uppercase">Is this pet right for you?</h4>
          <p>{{selectedPet.content.is_this_pet_right_for_you}}</p>
</template>
</p>
<a class="readMoreBtn" @click="toggleReadMore">
  <template v-if="readmore">READ LESS...</template>
  <template v-else>READ MORE...</template>
</a>
</div>
</div>
<div class="sidebar">
  <div v-for="pet in pets" class="pet" :key="pet.id" @click="updateSelectedPet(pet.id)" :class="{readmore:readmore}">
    <img
      :src="pet.image_url"
      alt="pet image"
      :class="{selected_image:pet.id != selectedPet.id}"
    >
    <h4>{{pet.name}}</h4>
  </div>
</div>
</div>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PetsIndex',
  data () {
    return {
      pets: [],
      selectedPetIndex: 1,
      selectedPet: {
        id: '1',
        name: 'No Pet',
        content: {
          highlight: 'lorem ipsom',
          description: ' delor.',
          carring_for_pet: 'no content',
          is_this_pet_right_for_you: 'no content'
        },
        image_url: 'no content',
        image_url_hd: 'no content'
      },
      readmore: false
    }
  },
  mounted () {
    this.getPets().then(res => {
      let pets = res.data.data
      this.selectedPet = pets[0]
      this.pets = pets
    })
  },
  methods: {
    toggleReadMore () {
      this.readmore = !this.readmore
    },
    updateSelectedPet (id) {
      this.selectedPetIndex = id - 1
      this.selectedPet = Object.assign({}, this.selectedPet, this.pets[id - 1])
      window.scrollTo({top: 100,
        left: 100,
        behavior: 'smooth'})
    },
    getPets () {
      return axios.get('http://localhost:3000/pets')
    }
  }
}
</script>

<style scoped>
  .wrapper {
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-flow: column;
  }
  .container-flex {
    display: flex;
    max-width: 1200px;
    margin: 0 auto;
    flex-flow: row wrap;
  }
  .container-flex > div {
    border-radius: 6px;
  }

  .selected_pet {
    display: flex;
    flex: 4;
    max-width: 1200px;
    padding: 12px;
    background: linear-gradient(to right, #79747491, white, #d2d2d28a);
    box-shadow: 4px 9px 12px 1px #989090;
  }

  .sidebar {
    flex: 1;
    margin-left: 12px;
    overflow: hidden;
    display: flex;
    justify-content: space-evenly;
    flex-flow: column;
    background: linear-gradient(to left, #79747491, white, #d2d2d28a);
    box-shadow: 4px 9px 12px 1px #989090;
    min-width: 210px;
  }

  .selected_pet > .image {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .selected_pet img {
    border-radius: 6px;
    max-height: 350px;
    padding: 6px;
    margin-top: 6px;
    transform: rotate(-2deg);
    box-shadow: 2px 2px 4px 1px #756f6f;
  }

  .selected_pet > .content {
    flex: 3;
    margin: 12px;
    display: flex;
    flex-flow: column wrap;
    justify-content: center;
    text-align: left;
  }

  .selected_pet > .content > .readMoreBtn {
    align-self: flex-end;
    cursor: pointer;
    color: green;
    font-weight: bold;
  }

  .selected_pet > .content .title {
    align-self: flex-start;
  }

  .sidebar .pet {
    display: flex;
    align-content: center;
    align-items: flex-start;
    margin: 6px;
  }

  .sidebar .pet:hover {
    zoom: 1.001;
    transition: 0.4s ease;
    cursor: pointer;
  }

  .sidebar .pet img {
    max-height: 65px;
    padding: 3px;
    margin: 6px;
    border-radius: 12px;
    box-shadow: 2px 2px 4px 1px #756f6f;
    transform: rotate(2deg);
  }

  .sidebar .pet h4 {
    text-align: left;
    padding-left: 6px;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.2s;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }

  .columnFlex {
    flex-flow: column wrap;
  }

  .uppercase {
    text-transform: uppercase;
  }

  .sidebar .pet .selected_image {
    opacity: 0.6;
    zoom: 0.9;
  }

  .pet.readmore {
    flex-flow: column-reverse;
    align-items: center;
    align-content: center;
  }
  .pet.readmore img {
    max-height: 85px;
  }

  .pet.readmore h4 {
    margin: 12px;
  }

  @media (max-width: 1000px) {
    .selected_pet {
      flex-flow: column;
    }

  }

  @media (max-width: 800px) {
    .container-flex {
      flex-flow: column wrap;
      align-content: space-between;
    }

    .selected_pet {
      flex-flow: column;
      height: auto;
      margin-bottom: 36px;
      padding: 24px;
    }

    .sidebar {
      align-content: center;
    }

    .sidebar .pet {
      flex-flow: column;
      align-items: center;
    }

    .sidebar .pet img {
      max-height: 220px;
      padding: 10px;
      margin-top: 24px;
      border-radius: 12px;
    }

    .selected_pet img {
      max-height: 390px;
    }
  }
</style>
