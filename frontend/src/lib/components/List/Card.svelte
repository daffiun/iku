<script>
  import Icon from "./icon.svelte";
  export let data;
</script>

<div class="flex flex-col cursor-pointer group">
  <div class="relative overflow-hidden rounded-2xl">
    <img
      class="w-full h-60 object-cover group-hover:scale-[1.03] transition-transform duration-500 ease-in-out"
      src={data.image[0]}
      alt={data.name}
      on:error={(e) =>
        (e.target.src =
          "https://placehold.co/800x600/6b7280/ffffff?text=Image+Missing")}
    />

    <!-- <button
      class="absolute top-3 right-3 p-2 rounded-full shadow transition-transform duration-200 hover:scale-105"
      style="background: var(--color-bg); border:1px solid var(--color-divider);"
    >
      <Icon name="favorite" size={24} color="var(--color-muted)" />
    </button> -->
  </div>

  <div class="mt-3">
	<h2
      class="text-base font-semibold transition-colors"
      style="color: var(--color-text);"
    >
      {data.nama}
    </h2>
    <!-- <div
      class="inline-block text-xs font-medium rounded-full px-3 py-1 mb-2"
      style="background: var(--color-divider); color: var(--color-muted);"
    >
      {data.kategori_id}
    </div> -->

    

    <p
      class="text-sm mt-1 flex items-center"
      style="color: var(--color-muted);"
    >
      📍 {data.alamat}
    </p>

    <p class="text-sm mt-1" style="color: var(--color-muted);">
      ⏰ {data.waktu_buka} - {data.waktu_tutup}
    </p>

    <p
      class="text-sm mt-2 line-clamp-2"
      style="color: var(--color-text); opacity: .8"
    >
      {data.short_desc}
    </p>
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
