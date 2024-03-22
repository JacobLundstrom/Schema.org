<script>
  import { onMount } from 'svelte';

  let categories = [];
  let expandedCategories = new Set();

  onMount(async () => {
    const response = await fetch('/Schema.jsonld');
    const data = await response.json();
    categories = data['@graph'].map(item => ({
      id: item['@id'],
      type: item['@type'],
      label: item['rdfs:label'] || 'Namnlös kategori',
      comment: item['rdfs:comment'] || 'Ingen beskrivning tillgänglig',
      expanded: false
    }));
  });

  function toggleCategory(categoryId) {
    if (expandedCategories.has(categoryId)) {
      expandedCategories.delete(categoryId);
    } else {
      expandedCategories.add(categoryId);
    }
    expandedCategories = new Set(expandedCategories); // Uppdatera reaktivt
  }
</script>

<main class="container">
  <div class="left-panel">
    {#each categories as category}
      <div class="category">
        <div class="category-header" on:click={() => toggleCategory(category.id)}>
          <span>{category.label}</span>
          <button class="expand-category">{expandedCategories.has(category.id) ? '-' : '+'}</button>
        </div>
        {#if expandedCategories.has(category.id)}
          <div class="category-content">
            <p>{category.comment}</p>

          </div>
        {/if}
      </div>
    {/each}
  </div>
</main>











  

  
<style>

:global(body) {
    background-color: #afacac;
  }
  .container {
    margin-top: 50px;
    display: flex;
    justify-content: space-between;
    padding: 40px;
  }

  .left-panel {
    width: auto;
    max-width: 30%;
    margin-left: 22%;
    padding: 20px;
    border-radius: 8px;
    background-color: #F0F0F0;
  }

  .right-panel {
    margin-right: 17%;
    width: 30%;
    background-color: #F0F0F0;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    max-height: 350px;
  }

  .category {
    margin-bottom: 15px;
  }

  .category-header {
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
    border-bottom: 1px solid #333;
    font-size: large;
    width: auto;
  }

  .subcategory-header {
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
    margin-left: 20px;
    border-bottom: 1px solid #333;
  }

  .output {
  background-color: lightgray;
  margin-left: 40px;
  margin-top: 15px;
  border-radius: 4px;
  word-wrap: break-word;
  overflow: auto;
  }

  ul {
    list-style-type: none;
    padding-left: 0;
    margin-top: 10px;
  }

  li {
    cursor: pointer;
    padding: 8px 0;
  }

  button {
    font-size: 18px;
    padding: 4px 8px;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    background-color: #6060e0;
    color: #fff;
  }

  button:hover {
    background-color: #ddd;
    color:#333;
  }

  .remove-button {
    cursor: pointer;
    font-size: small;
    color: white;
    margin: 0%;
    padding: 3px;
    background-color: red;
  }

.expand-category {
 background-color: #6060e0;
 margin-left: 200px; 
}

.expand-subcategory {
  background-color: #6060e0;
 }


  input[type="text"] {
    width: calc(100% - 16px);
    padding: 8px;
    margin-top: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  select {
    width: 100%;
    padding: 8px;
    margin-top: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    cursor: pointer;
  }

li.output {
  padding-left: 10px;
}

</style>

  
  