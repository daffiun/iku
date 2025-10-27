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

    let showAlert = false;
    let type = 'individual';
    let lainnyaTypeInput = ''; 
    
    $: showlainnyaInput = type === 'lainnya';

    function selectPill(val_ID) {
        type = val_ID;
    }

    function handleSubmit(event) {
        event.preventDefault(); 
        showAlert = true; 
    }
</script>

<section id="contact_us" class="flex-center flex-col mt-12">
    <div class="title text-center text-5xl mb-12">
        <h2>Memiliki <span class="text-base">Pertanyaan?</span><br>Hubungi Kami.</h2>
    </div>
    {#if showAlert}
        <div class="bg-teal-50 border-t-2 border-teal-500 mb-6 rounded-lg p-4 max-w-2xl" role="alert" tabindex="-1" aria-labelledby="hs-bordered-success-style-label" transition:fly={{ x: -200, duration: 500, easing: quintOut }}>
            <div class="flex">
                <div class="shrink-0">
                    <span class="inline-flex justify-center items-center size-8 rounded-full border-4 border-teal-100 bg-teal-200 text-teal-800">
                        <svg class="shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                            <path d="M12 22c5.523 0 10-4.477 10-10S17.523 2 12 2 2 6.477 2 12s4.477 10 10 10z"></path>
                            <path d="m9 12 2 2 4-4"></path>
                        </svg>
                    </span>
                </div>
                <div class="ms-3">
                    <h3 id="hs-bordered-success-style-label" class="text-gray-800 font-semibold">Terimakasih telah mengirimkan pesan.</h3>
                    <p class="text-sm text-gray-700">Pesan Anda telah kami terima dan akan segera diproses oleh tim kami. Kami akan menghubungi Anda kembali dalam waktu 1-2 hari kerja.</p>
                </div>
            </div>
        </div>
    {/if}
    <form on:submit={handleSubmit} class="grid grid-cols-2 gap-x-14 gap-y-8 w-2xl max-w-6xl">
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