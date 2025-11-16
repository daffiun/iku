<script>
  import Icon from "./icon.svelte";

  export let searchTerm;
  export let activeCategory;
  export let executeSearch;

  let activeIndex = 0;

  const navigationItems = [
    {
      name: "Cari semua",
      icon: "search",
      category: "Cari semua",
      headline: "Cari apa yang kamu inginkan",
    },
    {
      name: "F&B",
      icon: "restaurant",
      category: "F&B",
      headline: "Disini ada makanan enak lhooo",
    },
    {
      name: "Bahan Baku",
      icon: "storefront",
      category: "Bahan Baku",
      headline: "Butuh bahan apa?",
    },
    {
      name: "Aksesoris & Fashion",
      icon: "apparel",
      category: "Aksesoris & Fashion",
      headline: "Fashion disini bagus-bagus loh",
    },
    {
      name: "Jasa",
      icon: "handyman",
      category: "Jasa",
      headline: "Butuh bantuan apa kawan?",
    },
  ];

  if (!activeCategory) {
    activeCategory = navigationItems[0].category;
  }

  function setActive(index) {
    activeIndex = index;
    activeCategory = navigationItems[index].category;
  }

  function handleEnter(event) {
    if (event.key === "Enter") executeSearch();
  }

  function getIconColor(index) {
    return activeIndex === index
      ? "var(--color-primary)"
      : "var(--color-muted)";
  }
</script>

<!--  WRAPPER -->
<div class="pb-4" style="background:var(--color-bg); color:var(--color-text)">
  <div class="max-w-4xl mx-auto py-6">
    <!-- HEADLINE -->
    <h1 class="text-4xl font-bold text-center mb-10">
      {navigationItems[activeIndex].headline}
    </h1>

    <!-- NAVIGATION -->
    <nav
      class="flex justify-center space-x-8 mb-8 overflow-x-auto whitespace-nowrap
                    border-b"
      style="border-color:var(--color-divider)"
    >
      {#each navigationItems as item, index}
        <button
          class="pb-3 px-1 flex items-center gap-1 font-medium transition duration-150"
          style="
                        color: {activeIndex === index
            ? 'var(--color-primary)'
            : 'var(--color-muted)'};
                        border-bottom: 2px solid {activeIndex === index
            ? 'var(--color-primary)'
            : 'transparent'};
                    "
          on:click={() => setActive(index)}
        >
          <Icon
            name={item.icon}
            size={24}
            fill={activeIndex === index ? 1 : 0}
            color={getIconColor(index)}
          />
          {item.name}
        </button>
      {/each}
    </nav>

    <!-- SEARCH BAR -->
    <div
      class="flex items-center p-2 rounded-full shadow-xl shadow-black/50 justify-between"
      style="background:var(--color-surface)"
    >
      <div
        class="flex items-center pl-4 flex-grow gap-2"
        style="color:var(--color-muted)"
      >
        <Icon name="search" size={24} color="var(--color-muted)" />

        <input
          class="p-3 bg-transparent focus:outline-none w-full"
          style="color:var(--color-text)"
          type="text"
          bind:value={searchTerm}
          placeholder="Cari di {navigationItems[activeIndex].name}..."
          on:keydown={handleEnter}
        />
      </div>

      <button
        class="font-bold py-2 px-6 rounded-full transition duration-150 shadow-md"
        style="background:var(--color-primary); color:black; border:1px solid var(--color-primary)"
        on:click={executeSearch}
      >
        Search
      </button>
    </div>
  </div>
</div>

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
