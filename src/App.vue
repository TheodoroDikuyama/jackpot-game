<script setup>
import {ref} from "vue";

const timePerIcon = 100;

const iconHeightDesktop = 116;

const count = 200,
    defaults = {
      origin: {y: 0.7},
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
    reel: "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/lista_ids.webp?alt=media&token=be53035c-57c0-41ba-80a2-1630e408af03",
    numIcons: 135,
    primaryColor: "#D59C00",
    background_image:
        "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/background-doradobet.webp?alt=media&token=fd0a3a55-5c6b-4052-99f9-82d0cefa2681",
    row_reverse: false,
  },
];

const ids = [
  2425343, 2571991, 2723321, 2809618, 2811055, 2859334, 2949968, 3058093,
  3097282, 3104362, 3147451, 3169333, 3377710, 3572499, 3972182, 4160989,
  4211245, 4345249, 4347736, 4349812, 4385137, 4471321, 4492522, 4532518,
  4832863, 4875010, 5045737, 5055472, 5346107, 5518320, 5552457, 5565162,
  5586944, 5587094, 5845083, 5860218, 5866170, 5876685, 5880756, 5884131,
  5896194, 5896746, 6000866, 6054854, 6184912, 6226297, 6239554, 6428054,
  6437280, 6454628, 6456824, 6467576, 6543855, 6589165, 6600460, 6609833,
  6610760, 6623302, 6635218, 6653752, 6749125, 6843450, 6872531, 6874403,
  6882177, 6889625, 6890799, 6894882, 6898359, 6898975, 6902509, 6902716,
  6905673, 6916022, 6930998, 6932072, 6937039, 6938011, 6944632, 6949906,
  6950341, 6963820, 6965299, 6971119, 6973349, 6975902, 6977715, 6979623,
  6984501, 6996238, 6998297, 7014430, 7026135, 7028708, 7046055, 7061115,
  7069702, 7070864, 7073663, 7079373, 7089795, 7099527, 7104964, 7110034,
  7110727, 7124030, 7125790, 7129886, 7163057, 7166087, 7166248, 7166302,
  7166840, 7169024, 7169653, 7174579, 7176363, 7176743, 7177736, 7180751,
  7183068, 7184973, 7187991, 7190607, 7191955, 7192002, 7192159, 7195054,
  7197878, 7198116, 7198160, 7206962, 7211941, 7229994, 7248437,
];

const roll = (reel) => {
  const winningIndex = ids.indexOf(5552457);
  const iconHeight =
      window.innerWidth >= 480 ? iconHeightDesktop : iconHeightMobile;
  const delta = winningIndex;

  return new Promise((resolve) => {
    const style = getComputedStyle(reel);
    const backgroundPositionY = parseFloat(style["background-position-y"]);

    setTimeout(() => {
      reel.style.transition = `background-position-y ${
          (3 + delta) * timePerIcon
      }ms cubic-bezier(.41,-0.01,.63,1.09)`;
      reel.style.backgroundPositionY = `${
          backgroundPositionY + delta * iconHeight
      }px`;
    }, 150);

    setTimeout(() => {
      reel.style.transition = "none";
      reel.style.backgroundPositionY = `${
          ((backgroundPositionY + delta * iconHeight) % lists[0].numIcons) *
          iconHeight
      }px`;
      resolve(winningIndex % lists[0].numIcons);
    }, (3 + delta) * timePerIcon + 150);
  });
};

const toggleActive = () => {
  isActive.value = !isActive.value;

  if (isActive.value) {
    roll(document.querySelector(".slots .reel"));
  }
};
</script>

<template>
  <div class = "relative w-screen h-screen overflow-hidden">
    <img :src = "lists[0].background_image" alt = "background mantenimiento" class = "w-full h-full absolute"/>

    <div :class = "`absolute mt-8 sm:mt-2 h-[60%] sm:h-screen flex flex-col items-center justify-evenly sm:top-0 top-1/3 ${
        lists[0].row_reverse ? 'sm:right-0' : 'sm:left-0'
      } w-full sm:w-1/2`">
      <div class = "relative sm:h-[510px] sm:w-[350px] h-[350px] w-[200px]">
        <img alt = "maquina tragamonedas"
             class = "absolute w-full h-full"
             src = "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/jackpot.webp?alt=media&token=f1629709-d7ff-48fd-a181-58ae561429a7"/>
        <!--Jackpot-->
        <div :class = "`slots w-[170px] h-[100px] sm:p-[30px] sm:w-[300px] sm:h-[180px] z-10 ${
            isWin1 ? 'win1' : ''
          } ${isWin2 ? 'win2' : ''}`">
          <svg class = "absolute top-2 right-[80px] sm:right-36"
               fill = "none"
               height = "8"
               viewBox = "0 0 42 8"
               width = "20%"
               xmlns = "http://www.w3.org/2000/svg">
            <ellipse cx = "21" cy = "4" fill = "white" rx = "21" ry = "4"/>
          </svg>
          <svg class = "absolute top-2 right-5"
               fill = "none"
               height = "8"
               viewBox = "0 0 97 8"
               width = "30%"
               xmlns = "http://www.w3.org/2000/svg">
            <ellipse cx = "48.5" cy = "4" fill = "white" rx = "48.5" ry = "4"/>
          </svg>
          <div class = "flex flex-row w-[140px] h-[80px] sm:w-[320px] sm:h-[130px] rounded-[14px] border-[7px] border-[#746F6F] z-10">
            <div class = "reel z-10"></div>
          </div>
          <svg class = "absolute bottom-0 left-0 -z-1"
               fill = "none"
               height = "80%"
               viewBox = "0 0 371 171"
               width = "100%"
               xmlns = "http://www.w3.org/2000/svg">
            <path d = "M80 12C56.8 -8.40002 17 3.49998 0 12V145C0 165.4 3 170.5 4.5 170.5H371C365.667 159.167 348 135.5 320 131.5C285 126.5 109 37.5 80 12Z"
                  fill = "#DE7819"
                  fill-opacity = "0.35"/>
          </svg>
          <svg class = "absolute right-1"
               fill = "none"
               height = "90%"
               viewBox = "0 0 8 160"
               width = "5%"
               xmlns = "http://www.w3.org/2000/svg">
            <ellipse cx = "4" cy = "80" fill = "white" rx = "80" ry = "4" transform = "rotate(90 4 80)"/>
          </svg>
        </div>
        <!--Jackpot-->
        <img alt = "palanca"
             class = "absolute top-[175px] -right-3 sm:top-[280px] sm:-right-6 w-[10%] cursor-default"
             src = "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/PalancaA.svg?alt=media&token=503631a6-d35a-4f47-b86d-6463c36178c1"/>
        <img :class = "`${
            isActive ? 'rotate-hor-bottom' : ''
          } cursor-pointer absolute sm:top-[99px] top-[70px] -right-[34px] sm:-right-[61px] w-1/6`"
             alt = "palanca"
             src = "https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/PalancaB.svg?alt=media&token=f5370023-bae6-4070-9f57-03142285026b"
             @click = "toggleActive"/>
      </div>
    </div>
    <div :class = "`absolute flex flex-col items-center mt-6 gap-2 sm:gap-5 sm:justify-center top-0 sm:h-screen  h-1/3  sm:top-0 ${
        lists[0].row_reverse ? 'sm:left-0' : 'sm:right-0'
      } sm:right-0 w-full sm:w-1/2`">
      <h1 :style = "{ color: lists[0].primaryColor }" class = "w-[80%] text-center font-extrabold text-7xl">
        {{ lists[0].christmas_msg }} {{ lists[0].name }} </h1>
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
  background-image: url(https://firebasestorage.googleapis.com/v0/b/jackpot-game-9c133.appspot.com/o/sorteo.webp?alt=media&token=9c67f5b1-4f76-41fb-913f-b3a972e0b93e);
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
  -webkit-animation: rotate-hor-bottom 0.8s cubic-bezier(0.645, 0.045, 0.355, 1) both;
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
