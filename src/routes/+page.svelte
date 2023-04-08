<script>
  import hero from '$lib/assets/hero.png'
  import Loading from '$lib/components/loading.svelte';
  import {fade, slide} from 'svelte/transition'
	import { onMount } from 'svelte';
  import { copy } from 'svelte-copy';

  let loading = true
  let processing = false

  let url = ''
  let lastUrl = ''
  let shortened = ''
  $: lastUrl != url ? shortened = '' : ''
  onMount(function(){
    loading = false
  })
  async function handleSubmit(){
    if (shortened.length > 0 && url === lastUrl){
      return
    }
    processing = true
    lastUrl = url
    const res = await fetch('https://api-ssl.bitly.com/v4/shorten', {
        method: 'POST',
        headers: {
            'Authorization': `Bearer ${import.meta.env.VITE_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ "long_url": `${lastUrl}`})
    }).then(res => res.json());
    shortened = res.link
    processing = false
  }
</script>


<div class="grid grid-cols-2 grid-rows-1 col-start-2 col-end-4 row-start-2 row-end-6 rounded-md bg-blue-200 drop-shadow-2xl">
  {#if !loading}    
    <form class="flex flex-col gap-3 m-auto p-5 w-4/5" on:submit|preventDefault={handleSubmit}>
      <input transition:slide class="placeholder-shown:border-gray-500 py-3 px-4 rounded-full outline-none focus:ring focus:ring-violet-300 delicious hover:drop-shadow-md" placeholder="https://" type="url" bind:value={url} required disabled={processing}>
      <input transition:fade class="py-3 px-4 rounded-full outline-none delicious hover:bg-green-200 hover:cursor-pointer {!shortened ? "hidden" : ''}" readonly bind:value={shortened} use:copy={shortened} data-toggle="tooltip" data-placement="top" title="Click to Copy!">
      <div class="flex flex-row justify-items-center">
        <button class="delicious rounded-full p-3 min-w-fit w-2/3 mx-auto {shortened.length > 1 && url == lastUrl ? "bg-slate-400" : "bg-lime-200 hover:bg-lime-300 hover:ring hover:ring-lime-300 hover:drop-shadow-md disabled:animate-pulse" }" disabled={processing} >✂️</button>
      </div>
    </form>
  {:else}
    <Loading />
  {/if}
  <img src="{hero}" alt="Super link shortener" class="m-auto h-full" draggable="false">
</div>


<style>
  .delicious {
    font-family: 'Delicious Handrawn', cursive;
  }
</style>