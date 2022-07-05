<script lang="ts">
  import Button from "./lib/Button.svelte";
  import JSConfetti from "js-confetti";

  const limit = 6;
  // Declare activities
  const activities = [
    {
      title: "Draw with chalk! (outside or in the pen)",
      emojis: ["🎨", "🌈", "🐠", "❤️", "💩", "🐶", "🦋", "🦄"],
    },
    {
      title: "Mario and Yoshi RC race!",
      emojis: ["🏎", "🏁", "🚗", "🏍", "🛵", "🚦"],
    },
    {
      title: "Race on the Mario Kart Hotwheels track!",
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
    { title: "Make something out of cardboard!", emojis: ["📦", "📎", "✂️"] },
    { title: "Build a fort!", emojis: ["🏰", "🎪", "⛺️"] },
    { title: "Play camping!", emojis: ["🏕", "⛺️", "🪵", "🔥", "🚣‍♀️"] },
    {
      title: "Animal doctor!",
      emojis: ["🩺", "👩🏼‍⚕️", "👨🏽‍⚕️", "🥼", "🐈", "🐕", "🐇", "🐴"],
    },
    {
      title: "Pick a toy from the crawl space!",
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

  function shuffleActivities() {
    shuffledActivities = shuffleArray([...activities]).slice(0, limit);
    highlightedActivityIndex = null;
    selectedActivityIndex = null;
  }

  shuffleActivities();

  function spin({ velocity = 3000, decay = 1.005, friction = 35 }) {
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
          <p class="emojis">{activity.emojis.join(" ")}</p>
        </div>
      </li>
    {/each}
  </ul>
  <div class="controls">
    <Button label="Shuffle" on:click={() => shuffleActivities()} />
    <Button label="Spin" on:click={() => spin({})} />
  </div>
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
    gap: 3vmin;
    margin: 5vmin;
    padding: 0;
  }

  li {
    list-style-type: none;
    position: relative;
  }

  li.selected {
    animation-duration: 2s;
    animation-name: flipContainer;
    animation-fill-mode: forwards;
  }

  li .front,
  li .back {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    padding: 2rem;
    border: 1px solid grey;
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

  li.highlighted .front {
    background: yellow;
  }

  li.selected .front {
    background: yellow;
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
    background: lime;
  }

  .back {
    transform: rotateY(180deg);
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

  @keyframes flipContainer {
    0% {
      transform: scale(1);
    }
    30% {
      transform: scale(1.5);
    }

    100% {
      transform: scale(1);
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
    margin: 0 5vmin 5vmin;
  }

  p {
    max-width: 14rem;
    margin: 1rem auto;
    line-height: 1.35;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }

    p {
      max-width: none;
    }
  }
</style>
