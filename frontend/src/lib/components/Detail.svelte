<script>
  import { onMount } from 'svelte';
  import data from '$lib/assets/dummy/data.json';

  export let params;

  let umkm;

  onMount(() => {
    const id = parseInt(params.id);
	console.log(id);
    umkm = data.find(item => item.id === id);
  });
</script>

{#if !umkm}
  <div class="loading">
    <p>UMKM tidak ditemukan.</p>
  </div>
{:else}
  <div class="container">
    <header>{umkm.nama}</header>

    <div class="gallery">
      {#each umkm.image as img}
        <img src={img} alt={"Foto " + umkm.nama} />
      {/each}
    </div>

    <div class="content">
      <h2>{umkm.nama}</h2>
      <p class="short-desc">{umkm.short_desc}</p>

      <div class="desc">{umkm.desc}</div>

      <div class="info">
        <p><strong>Alamat:</strong> {umkm.alamat}</p>
        <p><strong>Jam Buka:</strong> {umkm.waktu_buka} - {umkm.waktu_tutup}</p>
        <p><strong>Koordinat:</strong> {umkm.coor.lat}, {umkm.coor.lng}</p>
      </div>

      <div class="social-media">
        {#each Object.entries(umkm.social_media) as [key, value]}
          <a
            href={"https://" + (value.includes("http") ? value : (key === "x" ? "x.com/" : key + ".com/") + value.replace("@", ""))}
            target="_blank"
            rel="noopener noreferrer">
            {key.toUpperCase()}
          </a>
        {/each}
      </div>

      <div class="produk">
        <h3>Produk</h3>
        <div class="produk-list">
          {#each umkm.produk as p}
            <div class="produk-item">
              <img src={p.img} alt={p.nama} />
              <p>{p.nama}</p>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
{/if}

<style>
  :global(body) {
    background: #121212;
    color: #fff;
    font-family: "Poppins", sans-serif;
  }

  .loading {
    text-align: center;
    margin-top: 4rem;
    color: #ff6600;
  }

  .container {
    max-width: 1000px;
    margin: 2rem auto;
    background: #1e1e1e;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 0 20px rgba(255, 102, 0, 0.25);
  }

  header {
    background: #ff6600;
    text-align: center;
    padding: 1rem;
    font-weight: bold;
    font-size: 1.5rem;
  }

  .gallery {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
  }

  .gallery img {
    width: 100%;
    height: 400px;
    object-fit: cover;
    scroll-snap-align: center;
  }

  .content {
    padding: 2rem;
  }

  h2 {
    color: #ff6600;
    margin-bottom: 0.3rem;
  }

  .short-desc {
    font-style: italic;
    opacity: 0.8;
  }

  .desc {
    margin: 1.5rem 0;
    line-height: 1.6;
  }

  .info {
    background: #2a2a2a;
    padding: 1rem 1.5rem;
    border-radius: 8px;
  }

  .social-media {
    margin-top: 1rem;
  }

  .social-media a {
    color: #ff6600;
    text-decoration: none;
    margin-right: 1rem;
    font-weight: 500;
  }

  .produk {
    margin-top: 2rem;
  }

  .produk h3 {
    color: #ff6600;
  }

  .produk-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 1rem;
  }

  .produk-item {
    background: #2a2a2a;
    border-radius: 8px;
    padding: 0.8rem;
    text-align: center;
    transition: 0.2s;
  }

  .produk-item:hover {
    transform: scale(1.03);
    box-shadow: 0 0 10px rgba(255, 102, 0, 0.3);
  }

  .produk-item img {
    width: 100%;
    height: 130px;
    object-fit: cover;
    border-radius: 6px;
  }

  .produk-item p {
    margin-top: 0.5rem;
    font-weight: 500;
  }

  @media (max-width: 600px) {
    .gallery img {
      height: 250px;
    }
    .content {
      padding: 1rem;
    }
  }
</style>

