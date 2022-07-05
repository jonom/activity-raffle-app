<script lang="ts">
  import Button from "./lib/Button.svelte";
  import JSConfetti from "js-confetti";

  const limit = 6;
  // Declare activities
  const activities = [
    {
      title: "Sidewalk (or pen) chalk!",
      emojis: ["🎨", "🌈", "🐠", "❤️", "💩", "🐶", "🦋", "🦄"],
    },
    {
      title: "Mario and Yoshi RC race!",
      emojis: ["🏎", "🏁", "🚗", "🏍", "🛵", "🚦"],
    },
    {
      title: "Mario Kart Hotwheels race!",
      emojis: ["🏎", "🏁", "🚗", "🏍", "🛵", "🚦"],
    },
    { title: "Bubble party!", emojis: ["🫧", "💦", "🥰"] },
    { title: "Garden scavenger hunt!", emojis: ["🪴", "🌸", "🍁", "🐞", "🐛"] },
    {
      title: "Learn to draw an animal!",
      emojis: ["🐶", "🐰", "🦊", "🐼", "🐷", "🦄", "🐻"],
    },
    {
      title: "Stickers & Stamps!",
      emojis: ["🐶", "🦄", "🌈", "🌸", "🦋", "🚗", "🚀", "⚽️"],
    },
    {
      title: "Paint a picture!",
      emojis: ["🎨", "🌈", "🐠", "❤️", "💩", "🐶", "🦋", "🦄", "🖼"],
    },
    { title: "Kinetic sand!", emojis: ["🏜", "🏰", "🐚", "🐠"] },
    {
      title: "Make a play-doh creature!",
      emojis: ["🐙", "🐍", "🦋", "🐝", "🧌"],
    },
    {
      title: "Build with Lego or Duplo!",
      emojis: ["🧱", "👷🏻‍♂️", "🏡", "🛠", "🦺", "🚧"],
    },
    {
      title: "Sandpit & water table!",
      emojis: ["💦", "💧", "🌊", "🏖", "⛱", "🏜"],
    },
    {
      title: "Playhouse café!",
      emojis: ["🥪", "🧁", "🍰", "🍪", "☕️", "🫖", "🥤", "🍦"],
    },
    { title: "Queens and Butlers!", emojis: ["👑", "👸", "💎"] },
    { title: "Verandah Santa!", emojis: ["🎅🏻", "🎄", "🎁"] },
    { title: "UNO / Pizza / Guess Who!", emojis: ["🎲", "🃏", "🎯", "♟"] },
    { title: "Find a Chirp activity!", emojis: ["🐥", "🎨", "🧶"] },
    {
      title: "Dress up as a ______!",
      emojis: ["🦺", "🛠", "👑", "🦹🏻‍♀️", "🦸🏼‍♂️", "👗", "👻", "🤖", "👽", "👮‍♀️"],
    },
    { title: "Make a cardboard creature!", emojis: ["📦", "📎", "✂️"] },
    { title: "Build a fort!", emojis: ["🏰", "🎪", "⛺️"] },
    { title: "Play camping!", emojis: ["🏕", "⛺️", "🪵", "🔥", "🚣‍♀️"] },
    {
      title: "Animal doctor!",
      emojis: ["🩺", "👩🏼‍⚕️", "👨🏽‍⚕️", "🥼", "🐈", "🐕", "🐇", "🐴"],
    },
    {
      title: "Pick a crawl space toy!",
      emojis: ["🧸", "🪀", "🚂", "🚗"],
    },
    { title: "Clicky tracks!", emojis: ["🚗", "🚙"] },
    { title: "Lite-Brite!", emojis: ["🎆", "🌌", "💡", "✨"] },
    { title: "Catch!", emojis: ["🫴🏼", "⚾️", "🥎", "🎾", "🖐🏼", "🤲🏻"] },
  ];

  const jsConfetti = new JSConfetti();

  // Randomize array in-place using Durstenfeld shuffle algorithm - https://stackoverflow.com/a/12646864
  function shuffleArray(array: any[]) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  let shuffledActivities = [];
  let highlightedActivityIndex: null | number = null;
  let selectedActivityIndex: null | number = null;
  let spinning = false;

  function shuffleActivities() {
    shuffledActivities = shuffleArray([...activities]).slice(0, limit);
    highlightedActivityIndex = null;
    selectedActivityIndex = null;
  }

  shuffleActivities();

  function spin({ velocity = 3000, decay = 1.005, friction = 35 }) {
    if (!spinning) spinning = true;
    selectedActivityIndex = null;
    const time = 50000 / velocity;
    const nextVelocity = velocity / decay - friction;
    const last = nextVelocity < friction;
    window.setTimeout(() => {
      tick({ last });
      if (!last)
        spin({
          velocity: nextVelocity,
        });
    }, time);
  }

  function tick({ last = false }) {
    if (
      highlightedActivityIndex === null ||
      highlightedActivityIndex === limit - 1
    ) {
      highlightedActivityIndex = 0;
    } else {
      highlightedActivityIndex = highlightedActivityIndex + 1;
    }
    if (last) {
      spinning = false;
      selectedActivityIndex = highlightedActivityIndex;
      window.setTimeout(() => {
        jsConfetti.addConfetti({
          emojis:
            Object.values(shuffledActivities)[selectedActivityIndex].emojis,
          emojiSize: 200,
        });
      }, 300);
    }
  }
</script>

<main>
  <ul class="cards">
    {#each shuffledActivities as activity, i}
      <li
        class:highlighted={i === highlightedActivityIndex}
        class:selected={i === selectedActivityIndex}
      >
        <div class="front">
          {activity.emojis[0]}
        </div>
        <div class="back">
          <h3 class="title">{activity.title}</h3>
          <p class="emojis">{activity.emojis.slice(0, 5).join(" ")}</p>
        </div>
      </li>
    {/each}
  </ul>
  <div class="controls">
    <Button
      disabled={spinning}
      label="Shuffle"
      on:click={() => shuffleActivities()}
    />
    <Button disabled={spinning} label="Spin" on:click={() => spin({})} />
  </div>
  <div
    class="veil"
    class:veil-semi-visible={spinning}
    class:veil-visible={selectedActivityIndex !== null}
    on:click={() => shuffleActivities()}
  />
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  :global(html),
  :global(body) {
    height: 100%;
    margin: 0;
  }

  :global(body) {
    position: relative;
    background: rgb(230, 241, 248);
  }

  main {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: grid;
    grid-template-rows: 1fr auto;
    overflow: hidden;
  }

  ul {
    display: grid;
    grid-template-columns: repeat(3, minmax(auto, 1fr));
    gap: 5vmin;
    margin: 7vmin;
    padding: 0;
  }

  li {
    list-style-type: none;
    position: relative;
  }

  li.selected {
    animation-duration: 2s;
    animation-fill-mode: forwards;
  }

  li.selected:nth-child(1) {
    animation-name: flipContainer1;
  }

  li.selected:nth-child(2) {
    animation-name: flipContainer2;
  }

  li.selected:nth-child(3) {
    animation-name: flipContainer3;
  }

  li.selected:nth-child(4) {
    animation-name: flipContainer4;
  }

  li.selected:nth-child(5) {
    animation-name: flipContainer5;
  }

  li.selected:nth-child(6) {
    animation-name: flipContainer6;
  }

  li .front,
  li .back {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    padding: 2rem;
    background: rgb(203, 245, 255);
    border: 2px solid rgb(93, 207, 236);
    border-radius: 1rem;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    backface-visibility: hidden;
    text-align: center;
  }

  li .front {
    font-size: 10vmin;
  }

  li.highlighted .front,
  li.selected .front {
    background: rgb(244, 238, 131);
    border-color: rgb(234, 224, 28);
  }

  li.highlighted,
  li.selected {
    z-index: 1;
    transform: scale(1.05);
    transition: transform 0.3s;
  }

  li.selected .front,
  li.selected .back {
    animation-duration: 2s;
    animation-fill-mode: forwards;
  }
  li.selected .front {
    animation-name: flipFront;
  }
  li.selected .back {
    animation-name: flipBack;
  }

  li .back {
    transform: rotateY(180deg);
    background: white;
    border-color: rgb(202, 217, 226);
  }

  .back .title {
    font-size: 5vmin;
    margin: 0;
    line-height: 1.3;
  }

  .back .emojis {
    font-size: 5vmin;
    margin: 0.25em 0 0;
  }

  @keyframes flipContainer1 {
    100% {
      transform: translate(calc(100% + 3.5vmin), calc(50% + 3.5vmin))
        scale(1.75);
    }
  }

  @keyframes flipContainer2 {
    100% {
      transform: translate(0, calc(50% + 3.5vmin)) scale(1.75);
    }
  }

  @keyframes flipContainer3 {
    100% {
      transform: translate(calc(-100% - 3.5vmin), calc(50% + 3.5vmin))
        scale(1.75);
    }
  }

  @keyframes flipContainer4 {
    100% {
      transform: translate(calc(100% + 3.5vmin), calc(-50% - 3.5vmin))
        scale(1.75);
    }
  }

  @keyframes flipContainer5 {
    100% {
      transform: translate(0, calc(-50% - 3.5vmin)) scale(1.75);
    }
  }

  @keyframes flipContainer6 {
    100% {
      transform: translate(calc(-100% - 3.5vmin), calc(-50% - 3.5vmin))
        scale(1.75);
    }
  }
  @keyframes flipFront {
    0% {
      transform: perspective(100vh) rotateY(0);
    }

    100% {
      transform: perspective(100vh) rotateY(180deg);
    }
  }
  @keyframes flipBack {
    0% {
      transform: perspective(100vh) rotateY(180deg);
    }

    100% {
      transform: perspective(100vh) rotateY(360deg);
    }
  }

  .controls {
    margin: 0 7vmin 7vmin;
    text-align: center;
  }

  .controls :global(button) {
    margin: 0 0.3em;
  }

  p {
    max-width: 14rem;
    margin: 1rem auto;
    line-height: 1.35;
  }

  .veil {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: rgba(3, 0, 47, 0.35);
    opacity: 0;
    transition: opacity 1s ease;
    pointer-events: none;
  }

  .veil-semi-visible {
    opacity: 0.3;
  }

  .veil-visible {
    opacity: 1;
    pointer-events: all;
  }
</style>
