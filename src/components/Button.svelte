<script>
  export let negative;
  export let isDisabled;
</script>
<style>
  button{
    cursor: pointer;
    border-radius: 0.25rem;
    text-transform: uppercase;
  }

  button {
    font-family: inherit;
    font-size: inherit;
    -webkit-padding: 0.4em 0;
    padding: 0.4em;
    margin: 0 0 0.5em 0;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .default {
    background-color:  #0064c8e0;
    color: #FFF;
    border: 1px solid  #0064c8e0 ;
  }

  .negative {
    background-color:  #6c7074e0;
    color: #FFF;
    border: 1px solid  #6c7074e0 ;
  }

  .disabled {
    background-color:  #c1bcbce4;
    color: #000000;
    border: 1px solid  #c1bcbce4 ;
    cursor: default;
  }

  button {
    color: #333;
    background-color: #f4f4f4;
    outline: none;
    cursor: pointer;
  }

  button:disabled {
    color: #999;
  }

  button:not(:disabled):active {
    background-color: #ddd;
  }

  button:focus {
    border-color: #666;
  }  
</style>
  
<button class="{negative ? 'negative' : 'default'} {isDisabled ? 'disabled' : ''}" on:click disabled={isDisabled}>
  <slot/>
</button>