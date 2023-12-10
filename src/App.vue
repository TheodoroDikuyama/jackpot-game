<script setup>
import { onMounted, ref } from "vue";

const numIcons = 121;
const timePerIcon = 50;
const indexes = ref([0, 0, 0, 0]);

const iconWidthDesktop = 58 * 4;
const iconHeightDesktop = 116;
const iconWidthMobile = 30;
const iconHeightMobile = 66;

const isActive = ref(false);
const isWin1 = ref(false);
const isWin2 = ref(false);

const lists = [
  {
    name: "VS",
    christmas_msg: "Feliz Navidad",
    reel: "./assets/images/reel1.png",
    numIcons: 114,
    primaryColor: "#D59C00",
    background_image: "./assets/images/background-doradobet.png",
    row_reverse: false,
  },
  {
    name: "Quota",
    christmas_msg: "Feliz Navidad",
    reel: "./assets/images/reel2.png",
    numIcons: 28,
    primaryColor: "#ABC90B",
    background_image: "./assets/images/background-lotosport.png",
    row_reverse: true,
  },
  {
    name: "VS - Quota",
    christmas_msg: "Feliz Navidad",
    reel: "./assets/images/reel3.png",
    numIcons: 121,
    primaryColor: "#FFD028",
    background_image: "./assets/images/background-ecuabet.png",
    row_reverse: true,
  },
];
const selectedList = ref(lists[0]); // default list

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
      resolve(delta % numIcons);
    }, (3 + 1 * delta) * timePerIcon + offset * 150);
  });
};

const rollAll = () => {
  const reelsList = document.querySelectorAll(".slots .reel");
  Promise.all([...reelsList].map((reel, i) => roll(reel, i))).then((deltas) => {
    deltas.forEach(
      (delta, i) => (indexes.value[i] = (indexes.value[i] + delta) % numIcons)
    );
    isActive.value = false;
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
          src="./assets/images/jackpot.webp"
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
          src="./assets/images/PalancaA.svg"
        />
        <img
          :class="`${
            isActive ? 'rotate-hor-bottom' : ''
          } cursor-pointer absolute sm:top-[99px] top-[70px] -right-[34px] sm:-right-[61px] w-1/6`"
          alt="palanca"
          src="./assets/images/PalancaB.svg"
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
  background-image: url(../src/assets/images/reel3.png);
  @media screen and (max-width: 480px) {
    background: url(../src/assets/images/reel1.png);
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
