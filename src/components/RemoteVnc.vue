<script setup>
import { ref, defineProps } from "vue";
import ArrowCircleIcon from "./icon/ArrowCircleIcon.vue";
import RefreshIcon from "./icon/RefreshIcon.vue";

const isUp = ref(true);
const isRotate = ref(false);
const isActive = ref(false);
const vncIframe = ref(null);

const props = defineProps(["vncLink", "indexLink", "activeLink"]);

function reloadIframe() {
  isRotate.value = true;
  setTimeout(() => {
    isRotate.value = false;
  }, 1000);

  if (vncIframe.value) {
    vncIframe.value.src += "";
  }
}
</script>

<template>
  <section
    class="container-vnc"
    :id="`iframe${indexLink}`"
    :class="{ active: indexLink === activeLink, up: isUp }"
  >
    <iframe class="vnc-remote" :src="vncLink" ref="vncIframe"></iframe>

    <div class="vnc-barrier" :class="{ up: !isUp }"></div>

    <div class="menu">
      <div class="tab" @click="reloadIframe">
        <RefreshIcon
          class="icon"
          :class="{ rotate: isRotate, play: isRotate }"
        />
      </div>
      <div class="tab" @click="isUp = !isUp">
        <ArrowCircleIcon class="icon" :class="{ up: isUp }" />
      </div>
    </div>
  </section>
</template>

<style scoped>
.container-vnc {
  width: 900px;
  height: 550px;
  border: 2px solid #f0f0f0;
  position: relative;
  overflow: hidden;
  transition: 0.5s ease;
}

.container-vnc.active {
  border-color: salmon;
}

.container-vnc.up {
  height: 137px;
  /* height: 550px; */
}

.vnc-remote {
  width: 100%;
  height: 550px;
  border: none;
  margin: none;
  background-color: blueviolet;
  position: absolute;
  bottom: 0;
}

.vnc-barrier {
  width: 100%;
  /* height: 460px; */
  height: 47px;
  background: white;
  position: absolute;
  top: 0px;
  transform-origin: bottom;
  transition: transform 0.5s ease;
}

.vnc-barrier::after,
.vnc-barrier::before {
  display: block;
  content: "";
  height: 90px;
  background: white;
}

.vnc-barrier::after {
  width: 187px;
  position: absolute;
  bottom: -90px;
}

.vnc-barrier::before {
  width: 202px;
  right: 0;
  position: absolute;
  bottom: -90px;
}

.vnc-barrier.up {
  transform: translateY(-550px);
}

.menu {
  width: 40px;
  height: 80px;
  position: absolute;
  bottom: 10px;
  left: 10px;
}

.menu.tab {
  width: 100%;
  height: 50%;
}

.tab:hover {
  transform: scale(1.2);
  cursor: pointer;
}

.icon {
  color: salmon;
  transition: transform 0.5s ease;
}

.icon.up {
  transform: rotate(180deg);
}

.icon.rotate {
  animation: none;
  /* Menonaktifkan animasi awal */
}

.icon.rotate.play {
  animation: rotateAnimation 1s linear forwards;
}

@keyframes rotateAnimation {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
</style>
