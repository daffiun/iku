<script>
  import Search from "./Search.svelte";
  import Card from "./Card.svelte";
  import { onMount } from "svelte";
  import data from "$lib/assets/dummy/umkm.json";

  let allDataItems = data;
  let filteredDataItems = [];
  let inputSearchTerm = "";
  let currentSearchTerm = "";
  let activeCategory = "Cari semua";

  let isLoading = false;
  let error = null;

  const categoryMap = {
    "1": "F&B",
    "2": "BB",
    "3": "ANF",
    "4": "JASA",
    "5": "LAINNYA",
  };

  function filterData(term, category) {
    const lowerTerm = term.trim().toLowerCase();
    const lowerCategory = category.toLowerCase();

    let categoryFiltered = allDataItems.filter((item) => {
      const itemCategoryName = categoryMap[item.category_id]?.toLowerCase();
      
	  if (lowerCategory === "cari semua") return true;
      return itemCategoryName == lowerCategory;
    });

    if (lowerTerm) {
      return categoryFiltered.filter((item) => {
        const itemCategoryName =
          categoryMap[item.category_id]?.toLowerCase() ?? "";

        return (
          item.nama?.toLowerCase().includes(lowerTerm) ||
          item.short_desc?.toLowerCase().includes(lowerTerm) ||
          itemCategoryName.includes(lowerTerm)
        );
      });
    }

    return categoryFiltered;
  }

  export function executeSearch() {
    currentSearchTerm = inputSearchTerm;
    filteredDataItems = filterData(currentSearchTerm, activeCategory);
  }

  onMount(() => {
    executeSearch();
  });

  $: {
    if (!isLoading && allDataItems.length > 0) {
      filteredDataItems = filterData(currentSearchTerm, activeCategory);
    }
  }
</script>

<section class="min-h-screen p-8 py-30" style="background: var(--color-bg);">
  <div class="max-w-7xl mx-auto">
    <Search
      bind:searchTerm={inputSearchTerm}
      bind:activeCategory
      {executeSearch}
    />

    <h2
      class="text-3xl font-bold mt-10 mb-6 border-b pb-2"
      style="color: var(--color-text); border-color: var(--color-divider)"
    >
      Rekomendasi Terbaik Hari Ini ({filteredDataItems.length} Tempat)
    </h2>

    {#if isLoading}
      <p style="color: var(--color-muted)">Loading...</p>
    {:else if error}
      <p style="color: var(--color-primary); font-weight:bold">{error}</p>
    {:else if filteredDataItems.length === 0}
      <p style="color: var(--color-muted)">
        Maaf, tidak ditemukan hasil untuk
        <b>"{currentSearchTerm || "pencarian apa pun"}"</b>
        pada kategori
        <b>"{activeCategory}"</b>.
      </p>
    {:else}
      <div class="grid gap-8 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
        {#each filteredDataItems as item (item.id || item.name)}
            <a href={`umkm/${item.id}`}>
                <Card data={item} />
            </a>
        {/each}
      </div>
    {/if}
  </div>
</section>

<style>
  :root {
    --color-primary: #ff6600;
    --color-bg: #121212;
    --color-surface: #1f1f1f;
    --color-divider: #2a2a2a;
    --color-text: #ffffff;
    --color-muted: rgba(255, 255, 255, 0.45);
  }
</style>
