<script lang="ts">
  import Button from "./lib/Button.svelte";

  const limit = 9;
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

  function spin({ time = 25, decay = 1.01, friction = 150, cutoff = 1000 }) {
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
  <h1>I'm bored! Let's...</h1>

  <ul>
    {#each shuffledActivities as activity, i}
      <li
        class:highlighted={i === highlightedActivityIndex}
        class:selected={i === selectedActivityIndex}
      >
        {activity.title}
        {activity.emoji.join(" ")}
      </li>
    {/each}
  </ul>

  <Button label="Shuffle" on:click={() => shuffleActivities()} />
  <Button label="Spin" on:click={() => spin({})} />
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  img {
    height: 16rem;
    width: 16rem;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  li.highlighted {
    background: yellow;
  }

  li.selected {
    background: lime;
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
