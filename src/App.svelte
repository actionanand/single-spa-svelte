<script>
  import { onDestroy } from 'svelte';

  import { debounceTime, take } from 'rxjs/operators';

  import { getData, state$ } from '@actionanand/utility';

  import HobbyRoot from './components/HobbyRoot.svelte';

  export let name;

  let htmlContent = '';
  let paraNo;
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
    const event = new CustomEvent('svelte', { detail: {paraNo} });
    window.dispatchEvent(event);
	}

  onDestroy(() => {
    state$.next({data: ''});
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
  }

  .content {
    display: grid;
    max-width: 300px;
    grid-template-columns: 40% 1fr 3fr;
    grid-column-gap: 10px;
  }

  .content button {
  text-transform: uppercase;
  background: #49a6e9;
  color: hsl(205, 86%, 17%);
  padding: 0.375rem 0.75rem;
  letter-spacing: 1px;
  display: inline-block;
  transition: all 0.3s linear;
  font-size: 0.875rem;
  border: 2px solid #49a6e9;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border-radius: 0.5rem;
}

.content button:hover {
  color: #49a6e9;
  background: hsl(205, 86%, 81%);
  border-color: hsl(205, 86%, 81%);
}
</style>

<div class="svelte-container">
  {#if htmlContent}
    <section>Content from Vanilla JS page</section>

    <form class="content">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label>Paragraphs:</label>
      <input type="number" bind:value={paraNo} />
      <button on:click={handleClick} class="btn">Change</button>
    </form>
    <div>
      {@html htmlContent}
    </div>
  {:else}
    <!-- <section> <span class="title-span"> {name} </span> &nbsp; has been mounted successfully!</section> -->
    <HobbyRoot></HobbyRoot>
  {/if}
</div>
