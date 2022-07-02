<script lang="ts">
  import Button from "./lib/Button.svelte";

  const limit = 6;
  // Declare activities
  const activities = [
    { title: "Activity 1", emoji: ["🙉", "😃"] },
    { title: "Activity 2", emoji: ["🐛", "🙉"] },
    { title: "Activity 3", emoji: ["🐴", "🐌"] },
    { title: "Activity 4", emoji: ["💂‍♂️", "😄"] },
    { title: "Activity 5", emoji: ["🐼", "👷‍♂️"] },
    { title: "Activity 6", emoji: ["🦄", "🎑"] },
    { title: "Activity 7", emoji: ["👷", "🪀"] },
    { title: "Activity 8", emoji: ["🛻", "🤜"] },
    { title: "Activity 9", emoji: ["🐷", "👗"] },
    { title: "Activity 10", emoji: ["🐯", "🍒"] },
    { title: "Activity 11", emoji: ["🍈", "👮"] },
    { title: "Activity 12", emoji: ["🧑", "😇"] },
    { title: "Activity 13", emoji: ["🏛", "🌆"] },
    { title: "Activity 14", emoji: ["🖨", "🕶"] },
    { title: "Activity 15", emoji: ["🍎", "😔"] },
    { title: "Activity 16", emoji: ["🤩", "🐨"] },
    { title: "Activity 17", emoji: ["🍋", "🩰"] },
    { title: "Activity 18", emoji: ["😌", "🍑"] },
    { title: "Activity 19", emoji: ["🐤", "🌃"] },
    { title: "Activity 20", emoji: ["🧐", "👝"] },
    { title: "Activity 21", emoji: ["📲", "🛕"] },
    { title: "Activity 22", emoji: ["🌠", "🤏"] },
    { title: "Activity 23", emoji: ["🤲", "🪲"] },
    { title: "Activity 24", emoji: ["👏", "🙏"] },
    { title: "Activity 25", emoji: ["🧵", "😞"] },
  ];

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
          time: time * decay + (time / cutoff) * friction,
          decay,
          cutoff,
        });
    }, time);
  }

  function tick({ message = "tick", last = false }) {
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
        <div class="front" />
        <div class="back">
          {activity.title}
          {activity.emoji.join(" ")}
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
    gap: 1rem;
    margin: 0;
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
  }

  li.highlighted .front {
    background: yellow;
  }

  li.selected .front {
    background: lime;
    /* animation-duration: 2s;
    animation-name: flipOver; */
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

  .back {
    transform: rotateY(180deg);
    backface-visibility: hidden;
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
