<script>
  import { onDestroy } from 'svelte';

  import { debounceTime, take } from 'rxjs/operators';

  import { getData, state$ } from '@actionanand/utility';

  import HobbyRoot from './components/HobbyRoot.svelte';

  export let name;

  let htmlContent = '';
  let paraNo;
  let isResetHtmlContent = false;
  const appTitle = 'Single-Spa Svelte';

  document.title = appTitle;

  getData('/data').then((data) => {
      console.log('svelte ', data);
    });

  const utilSub = state$.pipe(debounceTime(0), take(1)).subscribe(async (resp) => {
    console.log('svelte ', resp);
    htmlContent = resp.data?.htmlPara;
    paraNo = resp.data?.paraNo;
  });

	function handleClick(e) {
    e.preventDefault();
    isResetHtmlContent = true;
    const event = new CustomEvent('svelte', { detail: {paraNo} });
    window.dispatchEvent(event);
	}

  onDestroy(() => {
    if (!isResetHtmlContent) {
      state$.next({data: ''});
    }
    // console.log(utilSub);
    utilSub.unsubscribe();
  });
</script>

<style scoped>
  section {
    font-size: 1.5rem;
    display: flex;
    justify-content: center;
    margin-bottom: 1.5rem;
  }

  .title-span {
    color: green;
  }

  .svelte-container{
    margin: 1rem 2rem;
    min-height: 70vh;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
    color: #333;
  }
  .content {
    display: grid;
    max-width: 300px;
    grid-template-columns: 40% 1fr 3fr;
    grid-column-gap: 10px;
  }

  .content button {
  text-transform: uppercase;
  background: green;
  color: rgb(4, 46, 4);
  padding: 0.375rem 0.75rem;
  letter-spacing: 1px;
  display: inline-block;
  transition: all 0.3s linear;
  font-size: 0.875rem;
  border: 2px solid green;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border-radius: 0.5rem;
}

.content button:hover {
  color: white;
  background: rgb(2, 80, 2);
  border-color: rgb(2, 80, 2);
}

.svelte-title {
  font-size: 1.8rem;
  font-weight: 600;
  color: green;
}

.content label {
  display: flex;
  justify-content: center;
  align-items: center;
}

.content input {
  border: none;
  border-radius: 0.5rem;
  font-size: 1.25rem;
  margin: 0 0.5rem;
  padding: 0.25rem 0.5rem;
  width: 4rem;
}

.content input:focus {
  outline: none;
  border-color: 1px solid green;
  -webkit-box-shadow: 0 0 5px green;
  box-shadow: 0 0 5px green;
}

.html-wrap {
  font-family: cursive;
  font-style: italic;
  color: green;
  text-align: justify;
}
</style>

<div class="svelte-container">
  {#if htmlContent}
    <section class="svelte-title">Content from Vanilla JS page</section>

    <form class="content">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label>Paragraphs:</label>
      <input type="number" bind:value={paraNo} />
      <button on:click={handleClick} class="btn">Change</button>
    </form>
    <div class="html-wrap">
      {@html htmlContent}
    </div>
  {:else}
    <!-- <section> <span class="title-span"> {name} </span> &nbsp; has been mounted successfully!</section> -->
    <HobbyRoot></HobbyRoot>
  {/if}
</div>
