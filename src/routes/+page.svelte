<script>
  let inputText = "I will be back tomorrow";
  import * as chrono from 'chrono-node';
  import nlp from "compromise";
  import datePlugin from 'compromise-dates';
  nlp.plugin(datePlugin);

  // @ts-ignore
  let compromiseOutput = null;
  // @ts-ignore
  let chronoOutput = null;

  function chronoParse() {
    const date = chrono.parseDate(inputText);
    chronoOutput = date || "No date found.";
  }

  function compromiseParse() {
    // @ts-ignore
    const doc = nlp(inputText).dates();
    const parsedDates = doc.get()[0];
    console.log(parsedDates);
    compromiseOutput = parsedDates || "No date found.";
  }

  $: if (inputText) {
    chronoParse();
    compromiseParse();
  }

</script>

<nav style="text-align: right; margin: 1rem 2rem;">
  <a href="https://github.com/gsidhu/date-parser-comparison" style="padding: 0.5rem; color: #111;">
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" class="bi bi-github" viewBox="0 0 16 16">
      <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27s1.36.09 2 .27c1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.01 8.01 0 0 0 16 8c0-4.42-3.58-8-8-8"/>
    </svg>
  </a>
</nav>
<main>
  <h1>Natural Language Processing for Dates</h1>
  <h2>What is this?</h2>
  <p>This is a web app to test different JavaScript-based libraries for converting human-readable dates into machine-readable dates.</p>
  <p>Currently it compares –</p>
  <ul>
    <li><a href="https://github.com/wanasit/chrono"><code>chrono</code></a></li>
    <li><a href="https://compromise.cool/"><code>compromise</code></a></li>
  </ul>

  <p>Want to suggest more? Open an Issue on <a href="https://github.com/gsidhu/date-parser-comparison">the GitHub repo.</a></p>

  <h2>How to use it?</h2>
  <h3>Input</h3>
  <!-- Input Text Here -->
  <div class="pure-g">
    <p>Enter a text with date here –</p>
    <form class="pure-form mx-auto">
      <input type="text" id="input-text" placeholder="the second monday of february" bind:value={inputText} />
    </form>
  </div>

  <p>Or pick one of these –</p>
  <div class="pure-g mx-auto text-center">
    <button class="pure-button" on:click={() => inputText = "I will be back tomorrow"}>I will be back tomorrow</button>
    <button class="pure-button" on:click={() => inputText = "Let's meet this week"}>Let's meet this week</button>
    <button class="pure-button" on:click={() => inputText = "I had a meeting last month"}>I had a meeting last month</button>
    <button class="pure-button" on:click={() => inputText = "She met him October last year"}>She met him October last year</button>
    <button class="pure-button" on:click={() => inputText = "This Friday from 13:00 - 16.00"}>This Friday from 13:00 - 16.00</button>
    <button class="pure-button" on:click={() => inputText = "5 days ago"}>5 days ago</button>
    <button class="pure-button" on:click={() => inputText = "the second monday of february"}>the second monday of february</button>
    <button class="pure-button" on:click={() => inputText = "next week"}>next week</button>
    <button class="pure-button" on:click={() => inputText = "2 weeks from now"}>2 weeks from now</button>
    <button class="pure-button" on:click={() => inputText = "next monday to friday"}>next monday to friday</button>
    <button class="pure-button" on:click={() => inputText = "q2 2022 to q3 2023"}>q2 2022 to q3 2023</button>
    <button class="pure-button" on:click={() => inputText = "christmas eve"}>christmas eve</button>
    <button class="pure-button" on:click={() => inputText = "2014-11-30T08:15:30-05:30"}>2014-11-30T08:15:30-05:30</button>
    <button class="pure-button" on:click={() => inputText = "april next year"}>april next year</button>
    <button class="pure-button" on:click={() => inputText = "from april next year to june"}>from april next year to june</button>
    <button class="pure-button" on:click={() => inputText = "Sat Aug 17 2013 18:40:39 GMT+0900 (JST)"}>Sat Aug 17 2013 18:40:39 GMT+0900 (JST)</button>
  </div>

  <!-- Output Here -->
  <h3>Output</h3>
  <table id="comparison-table" class="pure-table pure-table-horizontal pure-table-striped mx-auto w-80">
    <tbody>
      <tr>
        <td><strong>Chrono</strong></td>
        <td>{chronoOutput}</td>
      </tr>
      <tr>
        <td><strong>Compromise</strong></td>
        {#if compromiseOutput === "No date found."}
          <td>{compromiseOutput}</td>
        {:else}
          <td>
            {#each Object.keys(compromiseOutput) as key}
              {#if key === "duration"}
                <strong>duration:</strong> {"{"}
                  {#each Object.keys(compromiseOutput["duration"]) as subKey}
                    {subKey}: {compromiseOutput["duration"][subKey]}{#if subKey !== "minutes"},&nbsp;{/if}
                  {/each}
                {"}"}
              {:else}
              <strong>{key}</strong>: {compromiseOutput[key]}<br/>
              {/if}
            {/each}
          </td>
        {/if}
      </tr>
    </tbody>
  </table>

  <h3>Verdict</h3>
  <p>My personal opinion, of course.</p>
  <ul>
    <li><strong>Chrono</strong> is simpler and parses only the starting date-time.</li>
    <li><strong>Compromise</strong> can parse a complete date range and possibly work with more complex input. Although it struggles with some inputs unexpectedly. For e.g., <code>october two years from now</code> works but <code>october next year</code> does not.</li>
  </ul>
</main>

<style>
  main {
    padding: 1rem;
    margin: 1rem 5rem;
  }

  h1, h2 {
    color: rgb(212, 48, 8);
  }

  .pure-button {
    all: unset;
    cursor: pointer;
    color: rgb(212, 48, 8);
    text-decoration: underline;
    margin: 0.5rem;
  }

  .mx-auto {
    margin: auto;
  }

  #input-text {
    min-width: 80vw;
    padding: 1rem;
    font-size: 1.2rem;
    line-height: 1.5;
  }

  #comparison-table {
    font-family: monospace;
  }

  .w-80 {
    width: 80%;
  }
</style>