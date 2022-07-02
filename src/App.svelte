<script lang="ts">
  import logo from "./assets/svelte.png";
  import Counter from "./lib/Counter.svelte";

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

  function getRandomActivities(limit = 9) {
    return shuffleArray([...activities]).slice(0, limit);
  }

  let shuffledActivities = getRandomActivities();
</script>

<main>
  <img src={logo} alt="Svelte Logo" />
  <h1>Hello Typescript!</h1>

  <ul>
    {#each shuffledActivities as activity}
      <li>{activity.title}</li>
    {/each}
  </ul>

  <Counter />

  <p>
    Visit <a href="https://svelte.dev">svelte.dev</a> to learn how to build Svelte
    apps.
  </p>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme">SvelteKit</a> for
    the officially supported framework, also powered by Vite!
  </p>
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
