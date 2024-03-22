<script>
  import { onMount } from 'svelte';
  import Main from './Main.svelte';

  let schema = {};

  onMount(async () => {
    const response = await fetch('Schema.jsonld');
    schema = await response.json();
  });
</script>

<Main {schema} />
