<template>
  <router-view v-slot="slotProps">
    <transition name="route" mode="out-in">
      <component :is="slotProps.Component"></component>
    </transition>
  </router-view>

  <div class="container">
    <some-list></some-list>
  </div>

  <div class="container">
    <div class="block" :class="{ animate: blockAnimated }"></div>
    <button v-if="!blockAnimated" @click="animateBlock">Animate</button>
    <button v-else @click="resetBlock">Reset</button>
  </div>

  <div class="container">

    <transition name="paragr"
                @before-enter="beforeEnter"
                @enter="enter"
                @after-enter="afterEnter"
                @before-leave="beforeLeave"
                @leave="leave"
                @after-leave="afterLeave"
                @enter-cancelled="enterCancel"
                @leave-cancelled="leaveCancel"
    >
      <p v-if="paragraphVisible">This is only sometimes visible</p>
    </transition>

    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>

  <div class="container">
    <transition name="fade-button" mode="out-in">
      <button @click="showUsers" v-if="!usersVisible">Show Users</button>
      <button @click="hideUsers" v-else>Hide Users</button>
    </transition>
  </div>

  <base-modal @close="hideDialog" :show="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
import SomeList from '@/components/SomeList';

export default {
  components: {  SomeList },
  data () {
    return {
      blockAnimated: false,
      dialogIsVisible: false,
      paragraphVisible: false,
      usersVisible: false,
      enterInterval: null,
      leaveInterval: null,
      paraStartOpacity: 0,
      paraOpacity: null
    };
  },
  methods: {
    animateBlock () {
      this.blockAnimated = true;
    },
    resetBlock() {
      this.blockAnimated = false
    },
    toggleParagraph () {
      this.paragraphVisible = !this.paragraphVisible;
    },
    showDialog () {
      this.dialogIsVisible = true;
    },
    hideDialog () {
      this.dialogIsVisible = false;
    },
    showUsers () {
      this.usersVisible = true;
    },
    hideUsers () {
      this.usersVisible = false;
    },
    beforeEnter (element) {
      element.style.opacity = this.paraStartOpacity;
    },
    enter (element, done) {
      let round = 1;
      this.enterInterval = setInterval(() => {
        this.paraOpacity = round * 0.01
        element.style.opacity = this.paraOpacity;
        round++;
        if (round > (1 - this.paraStartOpacity) * 100) {
          this.paraStartOpacity = 1
          this.paraOpacity = null
          clearInterval(this.enterInterval);
          done()
        }
      }, 20);
    },
    enterCancel() {
      this.paraStartOpacity = this.paraOpacity
      this.paraOpacity = null
      clearInterval(this.enterInterval)
    },
    afterEnter () {},
    beforeLeave (element) {
      element.style.opacity = this.paraStartOpacity
    },
    leave (element, done) {
      let round = 1;
      this.leaveInterval = setInterval(() => {
        this.paraOpacity = this.paraStartOpacity - round * 0.01
        element.style.opacity  = this.paraOpacity
        round++;
        if (round > this.paraStartOpacity * 100) {
          this.paraStartOpacity = 0
          this.paraOpacity = null
          clearInterval(this.leaveInterval);
          done()
        }
      }, 20);
    },
    leaveCancel() {
      this.paraStartOpacity = this.paraOpacity
      this.paraOpacity = null
      clearInterval(this.leaveInterval)
    },
    afterLeave () {}
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}

.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /*transition: transform 0.3s ease-out;*/
}

.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.route-enter-active {
  animation: slide-scale 0.4s ease-out reverse;
}

.route-leave-active {
  animation: slide-scale 0.4s ease-in ;
}

.animate {
  animation: slide-scale 0.3s ease-out forwards ;
}

.fade-button-enter-active {
  animation: fade-in 0.4s ease-out
}

.fade-button-leave-active {
  animation: fade-in 0.2s ease-in reverse;
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
    opacity: 1
  }

  30% {
    transform: translateX(-120px) scale(1.1);
    opacity: 0.7;
  }

  100% {
    transform: translateX(-150px) scale(1);
    opacity: 0;
  }
}

@keyframes fade-in {
  from {
    opacity: 0
  }
  to {
    opacity: 1;
  }
}

</style>