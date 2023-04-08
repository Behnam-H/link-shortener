<script>
  import "../app.css";
</script>

<svelte:head>
  <title>Shord</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Delicious+Handrawn&display=swap" rel="stylesheet">
</svelte:head>

<div class="h-screen w-screen bg-orange-100 grid grid-cols-4 grid-rows-6">
  <slot />
</div>