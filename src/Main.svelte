<script>
  // Läser in sparade kategorier från localStorage eller använder standardvärden om inget finns sparad.
  let categories = JSON.parse(localStorage.getItem('categories')) || [
    {
      name: 'Category 1',
      expanded: false,
      subcategories: [
        { name: 'Subcategory 1', inputs: [], expanded: false },
        { name: 'Subcategory 2', inputs: [], expanded: false }
      ]
    },
    {
      name: 'Category 2',
      expanded: false,
      subcategories: [
        { name: 'Subcategory 1', inputs: [], expanded: false },
        { name: 'Subcategory 2', inputs: [], expanded: false }
      ]
    }
  ];

  let expandedCategories = [];
  let selectedCategory = '';
  let selectedSubCat = '';
  let newInput = '';

  // Funktion för att växla kategoriens expanderade tillstånd
  function toggleCategory(categoryName) {
    if (expandedCategories.includes(categoryName)) {
      expandedCategories = expandedCategories.filter(name => name !== categoryName);
    } else {
      expandedCategories = [...expandedCategories, categoryName];
    }
  }

  // Funktion för att växla underkategoriens expanderade tillstånd
  function toggleSubcategory(categoryName, subcategoryName) {
    const categoryIndex = categories.findIndex(cat => cat.name === categoryName);
    const subcategoryIndex = categories[categoryIndex].subcategories.findIndex(subcat => subcat.name === subcategoryName);
    categories[categoryIndex].subcategories[subcategoryIndex].expanded = !categories[categoryIndex].subcategories[subcategoryIndex].expanded;
  }

  // Funktion för att spara en ny input och uppdatera localStorage
  function saveInput(newInput) {
    if (!selectedCategory || !selectedSubCat) return;

    const selectedCategoryIndex = categories.findIndex(cat => cat.name === selectedCategory);
    const selectedSubcategoryIndex = categories[selectedCategoryIndex].subcategories.findIndex(subcat => subcat.name === selectedSubCat);

    if (selectedSubcategoryIndex !== -1) {
      categories[selectedCategoryIndex].subcategories[selectedSubcategoryIndex].inputs.push(newInput);
      // Uppdaterar kategorierna och sparar till localStorage
      categories = [...categories];
      localStorage.setItem('categories', JSON.stringify(categories));
    }
  }

  // Funktion för att radera en input och uppdatera localStorage
  function removeInput(categoryName, subcategoryName, inputIndex) {
    const categoryIndex = categories.findIndex(cat => cat.name === categoryName);
    const subcategoryIndex = categories[categoryIndex].subcategories.findIndex(subcat => subcat.name === subcategoryName);
    categories[categoryIndex].subcategories[subcategoryIndex].inputs.splice(inputIndex, 1);
    // Uppdaterar kategorierna och sparar till localStorage
    categories = [...categories];
    localStorage.setItem('categories', JSON.stringify(categories));
  }
</script>



  
<main class="container">
  <div class="left-panel">
    {#each categories as category}
      <div class="category">
        <div class="category-header" on:click={() => toggleCategory(category.name)}>
          <span>{category.name}</span>
          <button class="expand-category">{expandedCategories.includes(category.name) ? '-' : '+'}</button>
        </div>
        {#if expandedCategories.includes(category.name)}
          <ul>
            {#each category.subcategories as subcategory}
              <li>
                <div class="subcategory-header" on:click={() => toggleSubcategory(category.name, subcategory.name)}>
                  <span>{subcategory.name}</span>
                  <button class="expand-subcategory">{subcategory.expanded ? '-' : '+'}</button>
                </div>
                {#if subcategory.expanded}
                  {#each subcategory.inputs as input, index}
                    <li class="output">
                      {input}
                      <button class="remove-button" on:click={() => removeInput(category.name, subcategory.name, index)}>X</button>
                    </li>
                  {/each}
                {/if}
              </li>
            {/each}
          </ul>
        {/if}
      </div>
    {/each}
  </div>

  <div class="right-panel">
    <h2>Lägg till ny input</h2>
    <div>
      <label>Välj kategori:</label>
      <select bind:value={selectedCategory}>
        <option value="">Välj kategori</option>
        {#each categories as category}
          <option value={category.name}>{category.name}</option>
        {/each}
      </select>
    </div>
  
    {#if selectedCategory}
      <div>
        <label>Välj underkategori:</label>
        <select bind:value={selectedSubCat}>
          <option value="">Välj underkategori</option>
          {#each categories.find(cat => cat.name === selectedCategory).subcategories as subcategory}
            <option value={subcategory.name}>{subcategory.name}</option>
          {/each}
        </select>
      </div>
  
      {#if selectedSubCat}
        <div>
          <label>Ny input:</label>
          <input type="text" bind:value={newInput} placeholder="Ange ny input">
        </div>
  
        <button on:click={() => saveInput(newInput)}>Spara input</button>
      {/if}
    {/if}
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

  
  