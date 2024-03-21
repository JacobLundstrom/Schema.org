<script>
  let categories = [
    {
      name: 'To do',
      expanded: false,
      subcategories: [
        { name: 'Jacob', tasks: [], expanded: false },
        { name: 'Test', tasks: [], expanded: false }
      ]
    },
    {
      name: 'Completed',
      expanded: false,
      subcategories: [
        { name: 'Jacob', tasks: [], expanded: false },
        { name: 'Test', tasks: [], expanded: false }
      ]
    }
  ];

  let expandedCategories = [];
  let selectedCategory = '';
  let selectedPerson = '';
  let newTask = '';

  function toggleCategory(categoryName) {
    if (expandedCategories.includes(categoryName)) {
      expandedCategories = expandedCategories.filter(name => name !== categoryName);
    } else {
      expandedCategories = [...expandedCategories, categoryName];
    }
  }

  function toggleSubcategory(categoryName, subcategoryName) {
    const categoryIndex = categories.findIndex(cat => cat.name === categoryName);
    const subcategoryIndex = categories[categoryIndex].subcategories.findIndex(subcat => subcat.name === subcategoryName);
    categories[categoryIndex].subcategories[subcategoryIndex].expanded = !categories[categoryIndex].subcategories[subcategoryIndex].expanded;
  }

  function saveTask(newTask) {
    if (!selectedCategory || !selectedPerson) return;

    const selectedCategoryIndex = categories.findIndex(cat => cat.name === selectedCategory);
    const selectedSubcategoryIndex = categories[selectedCategoryIndex].subcategories.findIndex(subcat => subcat.name === selectedPerson);

    if (selectedSubcategoryIndex !== -1) {
      categories[selectedCategoryIndex].subcategories[selectedSubcategoryIndex].tasks.push(newTask);
      categories = [...categories];
    }
  }
  function removeTask(categoryName, subcategoryName, taskIndex) {
    const categoryIndex = categories.findIndex(cat => cat.name === categoryName);
    const subcategoryIndex = categories[categoryIndex].subcategories.findIndex(subcat => subcat.name === subcategoryName);
    categories[categoryIndex].subcategories[subcategoryIndex].tasks.splice(taskIndex, 1);
    categories = [...categories];
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
                <div class="person-header" on:click={() => toggleSubcategory(category.name, subcategory.name)}>
                  <span>{subcategory.name}</span>
                  <button class="expand-subcategory">{subcategory.expanded ? '-' : '+'}</button>
                </div>
                {#if subcategory.expanded}
                  {#each subcategory.tasks as task, index}
                    <li class="output">
                      {task}
                      <button class="remove-button" on:click={() => removeTask(category.name, subcategory.name, index)}>X</button>
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
    <h2>Lägg till ny uppgift</h2>
    <div>
      <label>Välj kategori:</label>
      <select bind:value={selectedCategory}>
        <option value="">Välj kategori</option>
        <option value="To do">To do</option>
        <option value="Completed">Completed</option>
      </select>
    </div>
  
    {#if selectedCategory}
      <div>
        <label>Välj person:</label>
        <select bind:value={selectedPerson}>
          <option value="">Välj person</option>
          {#each categories.find(cat => cat.name === selectedCategory).subcategories as subcategory}
            <option value={subcategory.name}>{subcategory.name}</option>
          {/each}
        </select>
      </div>
  
      {#if selectedPerson}
        <div>
          <label>Ny uppgift:</label>
          <input type="text" bind:value={newTask} placeholder="Ange ny uppgift">
        </div>
  
        <button on:click={() => saveTask(newTask)}>Spara uppgift</button>
      {/if}
    {/if}
  </div>
  
  </main>
  

  
<style>

:global(body) {
    background-color: #afacac;
  }
  .container {
    margin-top: 20px;
    display: flex;
    justify-content: space-between;
    padding: 40px;
  }

  .left-panel {
    width: auto;
    max-width: 30%;
    margin-left: 15%;
    padding: 20px;
    border-radius: 8px;
    background-color: #F0F0F0;
  }

  .right-panel {
    margin-right: 10%;
    width: 40%;
    background-color: #F0F0F0;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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

  .person-header {
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

  
  