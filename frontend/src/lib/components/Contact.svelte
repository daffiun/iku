<script>
    import { quintOut } from 'svelte/easing';
    import { fly } from 'svelte/transition';

    const checkboxSelection = [
        { text: 'Individual', val_ID: 'individual' },
        { text: 'UMKM', val_ID: 'umkm'},
        { text: 'Sponsor', val_ID: 'sponsor', },
        { text: 'Perusahaan', val_ID: 'perusahaan', },
        { text: 'Lainnya', val_ID: 'lainnya', }
    ];

    let type = 'individual';
    let lainnyaTypeInput = ''; 
    
    $: showlainnyaInput = type === 'lainnya';

    function selectPill(val_ID) {
        type = val_ID;
    }
</script>

<section id="contact_us" class="flex-center flex-col mt-32">
    <div class="title text-center text-5xl">
        <h2>Memiliki <span class="text-base">Pertanyaan?</span><br>Hubungi Kami.</h2>
    </div>
    <form action="" class="grid grid-cols-2 gap-x-14 gap-y-8 max-w-6xl mt-14">
        <label for="nama" class="col-span-2">
            <input type="text" id="nama" name="nama" class="pr-5" placeholder="Nama" required aria-label="Nama">
            <span class="required"></span>
            <span class="line"></span>
        </label>
        <label for="email">
            <input type="email" id="email" name="email" placeholder="Email" required aria-label="Email" autocomplete="email">
            <span class="required"></span>
            <span class="line"></span>
        </label>
        <label for="no_telp">
            <input type="text" id="no_telp" name="no_telp" placeholder="No. Telepon" required aria-label="No. Telepon">
            <span class="required"></span>
            <span class="line"></span>
        </label>
        <fieldset class="checkbox-group col-span-2">
            <legend class="text-light-dark">
                Kategori
                <span class="required"></span>
            </legend>
            <span class="required"></span>
            <div class="items flex gap-2 mt-3">
                {#each checkboxSelection as item}
                    <button class="type-pill rounded-2xl" 
                        class:active={type === item.val_ID}
                        on:click={() => selectPill(item.val_ID)}
                        aria-pressed={type === item.val_ID}
                        type="button">
                        {item.text}
                    </button>
                {/each}
            </div>

            {#if showlainnyaInput}
                <div class="mt-3" transition:fly={{ y: -10, duration: 500, easing: quintOut }}>
                    <label for="lainnyaType" class="input-label">
                        <input 
                            type="text" 
                            id="lainnyaType" 
                            name="lainnyaType" 
                            placeholder="Masukan Kategori Spesifik" 
                            bind:value={lainnyaTypeInput}
                            required
                            aria-label="Masukan Kategori Spesifik"
                        >
                        <span class="required"></span>
                        <span class="line"></span>
                    </label>
                </div>
            {/if}
        </fieldset>
        <div class="col-span-2">
            <div class="flex flex-col">
                <label for="pesan">Pesan</label>
                <textarea class="w-full" name="pesan" id="pesan"></textarea>
            </div>
        </div>
        <div class="col-span-2">
            <button type="submit" class="bg-transparant px-14 relative float-right borderborder-base text-base bg-base btn-nav py-1 btn-clip">
                <span class="stroke"></span>
                <span class="text-sm text-white whitespace-nowrap">KIRIM</span>
            </button>
        </div>
    </form>
</section>
