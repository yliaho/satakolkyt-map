<template>
  <div class="home">
    <div class="map-container">
      <div class="dimmer" v-if="showOverlay" @click="toggleOverlay" />
      <map-view v-if="showMap" @shore-click="populateSelectedShoreData" />
      <section v-else class="initial">
        <h1>Avataanko Karttanäkymä?</h1>
        <button @click="() => (showMap = true)">Avaa</button>
      </section>
      <transition name="overlayPop">
        <div
          v-if="showOverlay && selectedShoreData"
          class="overlay-box-wrapper"
        >
          <overlay-box>
            <shore-info :data="selectedShoreData" />
          </overlay-box>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import OverlayBox from '@/components/OverlayBox'
import ShoreInfo from '@/components/ShoreInfo'
import MapView from '@/components/MapView'

export default {
  name: 'home',

  data() {
    return {
      // Map
      showMap: false,
      startMapOnMounted: false,
      // Overlay box
      showOverlay: false,
      dimBackground: true,
      // Selected Shore
      selectedShoreData: null
    }
  },

  components: {
    MapView,
    ShoreInfo,
    OverlayBox
  },

  methods: {
    initMap() {
      this.$nextTick(() => {
        this.showMap = this.startMapOnMounted
      })
    },

    toggleOverlay() {
      if (!this.selectedShoreData) {
        this.showOverlay = false
      }

      this.showOverlay = !this.showOverlay
    },

    populateSelectedShoreData(data) {
      this.selectedShoreData = data
      this.toggleOverlay()
    }
  },

  mounted() {
    this.initMap()
  },

  beforeDestroy() {
    this.showMap = this.startMapOnMounted
  }
}
</script>

<style lang="scss" scoped>
.home {
  height: 100%;
}

.dimmer {
  position: fixed;
  z-index: 50;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
}

.map-container {
  height: 100%;
}

.initial {
  background-color: #eae6e0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: #4e4a45;

  h1 {
    font-size: 2rem;
    font-weight: bold;
    margin-bottom: 2rem;
  }

  button {
    width: 20ex;
  }
}

.overlay-box-wrapper {
  position: fixed;
  bottom: 0;
  z-index: 100;
  width: 100vw;
}

.overlayPop-enter-active {
  transition: transform 0.3s;
}

.overlayPop-leave-active {
  transition: all 0.5s;
}
.overlayPop-enter /* .fade-leave-active below version 2.1.8 */ {
  transform: translateY(-1em);
  opacity: 0;
}

.overlayPop-leave-to {
  transform: translateY(1em);
  opacity: 0;
}
</style>
