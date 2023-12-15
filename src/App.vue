<script setup>
import { ref, watchEffect } from "vue";
const timePerIcon = 50;
const indexes = ref([0, 0, 0, 0]);

const iconHeightDesktop = 116;

const count = 200,
  defaults = {
    origin: { y: 0.7 },
  };

function fire(particleRatio, opts) {
  confetti(
    Object.assign({}, defaults, opts, {
      particleCount: Math.floor(count * particleRatio),
    })
  );
}

const isActive = ref(false);
const isWin1 = ref(false);
const isWin2 = ref(false);

const lists = [
  {
    name: "VS",
    christmas_msg: "Feliz Navidad",
    reel: "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/reel1.webp?alt=media&token=24ba37e4-bbf4-46d6-a12a-d498340f2225",
    numIcons: 114,
    primaryColor: "#D59C00",
    background_image:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/background-doradobet.webp?alt=media&token=fd0a3a55-5c6b-4052-99f9-82d0cefa2681",
    row_reverse: false,
    source_audio:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/Sonido_Virtual.mp3?alt=media&token=367e3bff-3237-447f-90ed-64283c706274",
  },
  {
    name: "Quota",
    christmas_msg: "Feliz Navidad",
    reel: "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/reel2.webp?alt=media&token=8ec07c42-3755-48e8-b2c6-6b78c7bc09c7",
    numIcons: 28,
    primaryColor: "#ABC90B",
    background_image:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/background-lotosport.webp?alt=media&token=1e9bdbe1-026e-47f9-9307-9a67076f0e61",
    row_reverse: true,
    source_audio:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/Sonido_Quota.mp3?alt=media&token=ab1d5064-b25d-4784-82e6-dc6f048432db",
  },
  {
    name: "VS - Quota",
    christmas_msg: "Feliz Navidad",
    reel: "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/reel3.webp?alt=media&token=cf21a38d-ac6d-43c8-b47a-283429a0cc27",
    numIcons: 113,
    primaryColor: "#FFD028",
    background_image:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/background-ecuabet.webp?alt=media&token=48892d9f-b209-4380-b4ad-42bdb2f9839f",
    row_reverse: true,
    source_audio:
      "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/Virtual_Quota.mp3?alt=media&token=702ebeaa-04f8-4fda-b2cd-527f022492b4",
  },
];
const selectedList = ref(lists[0]); // default list

const isPlaying = ref(false);
const miAudio = ref(null);
watchEffect(() => {
  if (isActive.value && miAudio.value) {
    miAudio.value.play();
  } else {
    // not mounted yet, or the element was unmounted (e.g. by v-if)
  }
});
function changeList(listName) {
  const reel = document.querySelector(".slots .reel");
  if (listName === "VS") {
    selectedList.value = lists[0];
    reel.style.backgroundImage = `url(${selectedList.value.reel})`;
  } else if (listName === "Quota") {
    selectedList.value = lists[1];
    reel.style.backgroundImage = `url(${selectedList.value.reel})`;
  } else if (listName === "VS - Quota") {
    selectedList.value = lists[2];
    reel.style.backgroundImage = `url(${selectedList.value.reel})`;
  }
}

const roll = (reel, offset = 0) => {
  const iconHeight =
    window.innerWidth >= 480 ? iconHeightDesktop : iconHeightMobile;
  const delta =
    (offset + 2) * selectedList.value.numIcons +
    Math.round(Math.random() * selectedList.value.numIcons); // Number of icons to roll
  return new Promise((resolve) => {
    const style = getComputedStyle(reel);
    const backgroundPositionY = parseFloat(style["background-position-y"]);
    const targetBackgroundPositionY = backgroundPositionY + delta * iconHeight;
    const normTargetBackgroundPositionY =
      targetBackgroundPositionY % (selectedList.value.numIcons * iconHeight);
    setTimeout(() => {
      reel.style.transition = `background-position-y ${
        (3 + 1 * delta) * timePerIcon
      }ms cubic-bezier(.41,-0.01,.63,1.09)`;
      reel.style.backgroundPositionY = `${
        backgroundPositionY + delta * iconHeight
      }px`;
    }, offset * 150);

    setTimeout(() => {
      reel.style.transition = "none";
      reel.style.backgroundPositionY = `${normTargetBackgroundPositionY}px`;
      resolve(delta % selectedList.value.numIcons);
    }, (3 + 1 * delta) * timePerIcon + offset * 150);
  });
};

const rollAll = () => {
  const reelsList = document.querySelectorAll(".slots .reel");
  Promise.all([...reelsList].map((reel, i) => roll(reel, i))).then((deltas) => {
    deltas.forEach(
      (delta, i) =>
        (indexes.value[i] =
          (indexes.value[i] + delta) % selectedList.value.numIcons)
    );

    fire(0.25, {
      spread: 26,
      startVelocity: 55,
    });

    fire(0.2, {
      spread: 60,
    });

    fire(0.35, {
      spread: 100,
      decay: 0.91,
      scalar: 0.8,
    });

    fire(0.1, {
      spread: 120,
      startVelocity: 25,
      decay: 0.92,
      scalar: 1.2,
    });

    fire(0.1, {
      spread: 120,
      startVelocity: 45,
    });
    isWin1.value = true;
    isActive.value = false;
    setTimeout(() => {
      isWin1.value = false;
    }, 2000);
  });
};

const toggleActive = () => {
  isActive.value = !isActive.value;

  if (isActive.value) {
    rollAll();
  }
};
</script>

<template>
  <div class="relative w-screen h-screen overflow-hidden">
    <img
      :src="selectedList.background_image"
      alt="background mantenimiento"
      class="w-full h-full absolute"
    />

    <div
      :class="`absolute mt-8 sm:mt-2 h-[60%] sm:h-screen flex flex-col items-center justify-evenly sm:top-0 top-1/3 ${
        selectedList.row_reverse ? 'sm:right-0' : 'sm:left-0'
      } w-full sm:w-1/2`"
    >
      <div class="relative sm:h-[510px] sm:w-[350px] h-[350px] w-[200px]">
        <img
          alt="maquina tragamonedas"
          class="absolute w-full h-full"
          src="https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/jackpot.webp?alt=media&token=f1629709-d7ff-48fd-a181-58ae561429a7"
        />
        <!--Jackpot-->
        <div
          :class="`slots w-[170px] h-[100px] sm:p-[30px] sm:w-[300px] sm:h-[180px] z-10 ${
            isWin1 ? 'win1' : ''
          } ${isWin2 ? 'win2' : ''}`"
        >
          <svg
            class="absolute top-2 right-[80px] sm:right-36"
            fill="none"
            height="8"
            viewBox="0 0 42 8"
            width="20%"
            xmlns="http://www.w3.org/2000/svg"
          >
            <ellipse cx="21" cy="4" fill="white" rx="21" ry="4" />
          </svg>
          <svg
            class="absolute top-2 right-5"
            fill="none"
            height="8"
            viewBox="0 0 97 8"
            width="30%"
            xmlns="http://www.w3.org/2000/svg"
          >
            <ellipse cx="48.5" cy="4" fill="white" rx="48.5" ry="4" />
          </svg>
          <div
            class="flex flex-row w-[140px] h-[80px] sm:w-[320px] sm:h-[130px] rounded-[14px] border-[7px] border-[#746F6F] z-10"
          >
            <div class="reel z-10"></div>
          </div>
          <svg
            class="absolute bottom-0 left-0 -z-1"
            fill="none"
            height="80%"
            viewBox="0 0 371 171"
            width="100%"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M80 12C56.8 -8.40002 17 3.49998 0 12V145C0 165.4 3 170.5 4.5 170.5H371C365.667 159.167 348 135.5 320 131.5C285 126.5 109 37.5 80 12Z"
              fill="#DE7819"
              fill-opacity="0.35"
            />
          </svg>
          <svg
            class="absolute right-1"
            fill="none"
            height="90%"
            viewBox="0 0 8 160"
            width="5%"
            xmlns="http://www.w3.org/2000/svg"
          >
            <ellipse
              cx="4"
              cy="80"
              fill="white"
              rx="80"
              ry="4"
              transform="rotate(90 4 80)"
            />
          </svg>
        </div>
        <!--Jackpot-->
        <img
          alt="palanca"
          class="absolute top-[175px] -right-3 sm:top-[280px] sm:-right-6 w-[10%] cursor-default"
          src="https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/PalancaA.svg?alt=media&token=503631a6-d35a-4f47-b86d-6463c36178c1"
        />
        <img
          :class="`${
            isActive ? 'rotate-hor-bottom' : ''
          } cursor-pointer absolute sm:top-[99px] top-[70px] -right-[34px] sm:-right-[61px] w-1/6`"
          alt="palanca"
          src="https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/PalancaB.svg?alt=media&token=f5370023-bae6-4070-9f57-03142285026b"
          @click="toggleActive"
        />
      </div>
      <div class="flex justify-around w-1/2">
        <button
          class="bg-[#FFD028] text-white font-bold py-2 px-6 rounded-full"
          @click="changeList('VS')"
        >
          VS
        </button>
        <button
          class="bg-[#D59C00] text-white font-bold py-2 px-6 rounded-full"
          @click="changeList('Quota')"
        >
          Quota
        </button>
        <button
          class="bg-[#ABC90B] text-white font-bold py-2 px-6 rounded-full"
          @click="changeList('VS - Quota')"
        >
          VS - Quota
        </button>
      </div>
    </div>
    <div
      :class="`absolute flex flex-col items-center mt-6 gap-2 sm:gap-5 sm:justify-center top-0 sm:h-screen  h-1/3  sm:top-0 ${
        selectedList.row_reverse ? 'sm:left-0' : 'sm:right-0'
      } sm:right-0 w-full sm:w-1/2`"
    >
      <h1
        :style="{ color: selectedList.primaryColor }"
        class="w-[80%] text-center font-extrabold text-7xl"
      >
        {{ selectedList.christmas_msg }} {{ selectedList.name }}
      </h1>
      <audio ref="miAudio" style="display: none">
        <source :src="selectedList.source_audio" type="audio/mpeg" />
      </audio>
    </div>
  </div>
</template>

<style scoped>
.slots {
  position: relative;
  top: 42%;
  left: 6%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #ffc600;
  border-radius: 7px;
}

.slots.win1 {
  animation: win1 200ms steps(2, end) infinite;
}

.slots.win2 {
  animation: win2 200ms steps(2, end) infinite;
}

@keyframes win1 {
  0% {
    background: linear-gradient(45deg, orange 0%, yellow 100%);
    box-shadow: 0 0 80px orange;
  }
  100% {
    background: linear-gradient(45deg, grey 0%, lightgrey 100%);
    box-shadow: -2px 2px 3px rgba(0, 0, 0, 0.3);
  }
}

@keyframes win2 {
  0% {
    background: linear-gradient(45deg, lightblue 0%, lightgreen 100%);
    box-shadow: 0 0 80px lightgreen;
  }
  100% {
    background: linear-gradient(45deg, grey 0%, lightgrey 100%);
    box-shadow: -2px 2px 3px rgba(0, 0, 0, 0.3);
  }
}

.slots .reel {
  position: relative;
  width: 100%;
  height: 100%;
  border-right: 7px solid #746f6f;
  overflow: hidden;
  background-image: url(https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/reel1.webp?alt=media&token=24ba37e4-bbf4-46d6-a12a-d498340f2225);
  @media screen and (max-width: 480px) {
    background: url(https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/reel1.webp?alt=media&token=24ba37e4-bbf4-46d6-a12a-d498340f2225);
  }
  background-position: 0 0;
  background-repeat: repeat-y;
}

.slots .reel:last-child {
  border-right: none;
  border-bottom-right-radius: 7px;
  border-top-right-radius: 7px;
}

.slots .reel:first-child {
  border-bottom-left-radius: 7px;
  border-top-left-radius: 7px;
}

.linear-gradient {
  background: linear-gradient(
    180deg,
    #3b405b 0%,
    rgba(59, 64, 91, 0.72) 30%,
    rgba(59, 64, 91, 0.38) 62.81%,
    rgba(59, 64, 91, 0) 100%
  );
}

.rotate-hor-bottom {
  -webkit-animation: rotate-hor-bottom 0.8s cubic-bezier(0.645, 0.045, 0.355, 1)
    both;
  animation: rotate-hor-bottom 0.8s cubic-bezier(0.645, 0.045, 0.355, 1) both;
}

@-webkit-keyframes rotate-hor-bottom {
  0% {
    -webkit-transform: rotateX(0);
    transform: rotateX(0);
    -webkit-transform-origin: bottom;
    transform-origin: bottom;
  }
  100% {
    -webkit-transform: rotateX(360deg);
    transform: rotateX(360deg);
    -webkit-transform-origin: bottom;
    transform-origin: bottom;
  }
}

@keyframes rotate-hor-bottom {
  0% {
    -webkit-transform: rotateX(0);
    transform: rotateX(0);
    -webkit-transform-origin: bottom;
    transform-origin: bottom;
  }
  100% {
    -webkit-transform: rotateX(360deg);
    transform: rotateX(360deg);
    -webkit-transform-origin: bottom;
    transform-origin: bottom;
  }
}
</style>
