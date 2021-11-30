<template>
  <main>
    <h1 class="title">Grattis Niklas 30 år!</h1>

    <transition name="fade">
      <Countdown
        v-if="!finished"
        :deadlineISO="state.deadline.toISOString()"
        labelColor="white"
        mainFlipBackgroundColor="white"
        secondFlipBackgroundColor="#ccc"
        mainColor="#000"
        secondFlipColor="#000"
        countdownSize="1.5rem"
        :showLabels="false"
      />
    </transition>

    <p>
      <small>
        <i>
          {{ state.deadline.toLocaleString("se-sv") }}
        </i>
      </small>
    </p>

    <p>
      <a
        class="link"
        :href="finished ? state.finalGpsPos : state.initGpsPos"
        target="_blank"
      >
        <img src="./assets/pin.svg" alt="" />
        <span v-if="finished">Hoppas det smakar!</span>
        <span v-else>Preliminär plats*</span>
      </a>
    </p>
  </main>

  <footer>
    <p>
      <small>
        <i>
          Vi har bokat en middag för två. <br />
          Meddela mig om datum och tid inte passar.
        </i>
      </small>
    </p>
  </footer>
</template>

<script>
import { onBeforeMount, reactive, computed } from "@vue/runtime-core";
import { Countdown } from "vue3-flip-countdown";

export default {
  components: {
    Countdown,
  },
  setup() {
    const state = reactive({
      now: new Date(),
      deadline: new Date("2022-01-18T19:45:00"),
      initGpsPos: "https://goo.gl/maps/oG1KZgqSibkE2NJT7",
      finalGpsPos: "https://goo.gl/maps/ShvSbLFBYekCnaP56",
    });

    const finished = computed(
      () => state.now.getTime() > state.deadline.getTime()
    );

    onBeforeMount(() => {
      setInterval(() => {
        state.now = new Date();
      }, 1000);
    });

    return {
      state,
      finished,
    };
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

h1,
h2,
h3,
h4,
h5,
p {
  margin: 0;
}

a,
a:visited,
a:focus {
  color: white;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
  width: 100vw;
  background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)),
    url("./assets/forrest.webp");
  background-size: cover;
  background-position: center center;
  display: grid;
  grid-template-rows: 2fr 1fr;
  place-content: center;
  color: white;
}

.link {
  display: flex;
  justify-content: center;
  align-items: self-end;
  gap: 0.3rem;
}

main {
  display: grid;
  place-content: center;
  gap: 2rem;
}

.title {
  font-family: "Dancing Script", cursive;
  font-size: 2rem;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
