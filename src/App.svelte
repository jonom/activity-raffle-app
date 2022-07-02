<script lang="ts">
  import Button from "./lib/Button.svelte";
  import JSConfetti from "js-confetti";

  const limit = 6;
  // Declare activities
  const activities = [
    { title: "Activity 1", emojis: ["🙉", "😃"] },
    { title: "Activity 2", emojis: ["🐛", "🙉"] },
    { title: "Activity 3", emojis: ["🐴", "🐌"] },
    { title: "Activity 4", emojis: ["💂‍♂️", "😄"] },
    { title: "Activity 5", emojis: ["🐼", "👷‍♂️"] },
    { title: "Activity 6", emojis: ["🦄", "🎑"] },
    { title: "Activity 7", emojis: ["👷", "🪀"] },
    { title: "Activity 8", emojis: ["🛻", "🤜"] },
    { title: "Activity 9", emojis: ["🐷", "👗"] },
    { title: "Activity 10", emojis: ["🐯", "🍒"] },
    { title: "Activity 11", emojis: ["🍈", "👮"] },
    { title: "Activity 12", emojis: ["🧑", "😇"] },
    { title: "Activity 13", emojis: ["🏛", "🌆"] },
    { title: "Activity 14", emojis: ["🖨", "🕶"] },
    { title: "Activity 15", emojis: ["🍎", "😔"] },
    { title: "Activity 16", emojis: ["🤩", "🐨"] },
    { title: "Activity 17", emojis: ["🍋", "🩰"] },
    { title: "Activity 18", emojis: ["😌", "🍑"] },
    { title: "Activity 19", emojis: ["🐤", "🌃"] },
    { title: "Activity 20", emojis: ["🧐", "👝"] },
    { title: "Activity 21", emojis: ["📲", "🛕"] },
    { title: "Activity 22", emojis: ["🌠", "🤏"] },
    { title: "Activity 23", emojis: ["🤲", "🪲"] },
    { title: "Activity 24", emojis: ["👏", "🙏"] },
    { title: "Activity 25", emojis: ["🧵", "😞"] },
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

  function spin({ time = 25, decay = 1.005, friction = 300, cutoff = 1000 }) {
    selectedActivityIndex = null;
    window.setTimeout(() => {
      const last = time > cutoff;
      tick({ last });
      if (!last)
        spin({
          time: time * decay + ((time - 24) / cutoff) * friction,
          decay,
          cutoff,
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
      jsConfetti.addConfetti({
        emojis: Object.values(shuffledActivities)[selectedActivityIndex].emojis,
        emojiSize: 200,
      });
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
