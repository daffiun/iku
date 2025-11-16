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

<section id="contact_us" class="flex-center flex-col mt-32 px-4">
    <div class="title text-center text-4xl sm:text-5xl leading-tight">
        <h2>
            Memiliki <span class="text-base">Pertanyaan?</span><br>
            Hubungi Kami.
        </h2>
    </div>

    <form 
        action="" 
        class="grid grid-cols-1 sm:grid-cols-2 gap-6 sm:gap-x-14 sm:gap-y-8 max-w-4xl w-full mt-14"
    >
        <label for="nama" class="col-span-1 sm:col-span-2">
            <input type="text" id="nama" name="nama" class="pr-5" placeholder="Nama" required>
            <span class="required"></span>
            <span class="line"></span>
        </label>

        <label for="email" class="col-span-1">
            <input type="email" id="email" name="email" placeholder="Email" required autocomplete="email">
            <span class="required"></span>
            <span class="line"></span>
        </label>

        <label for="no_telp" class="col-span-1">
            <input type="text" id="no_telp" name="no_telp" placeholder="No. Telepon" required>
            <span class="required"></span>
            <span class="line"></span>
        </label>

        <fieldset class="checkbox-group col-span-1 sm:col-span-2">
            <legend class="text-light-dark">
                Kategori
                <span class="required"></span>
            </legend>

            <div class="items flex flex-wrap gap-2 mt-3">
                {#each checkboxSelection as item}
                    <button 
                        class="type-pill rounded-2xl px-4 py-2 text-sm"
                        class:active={type === item.val_ID}
                        type="button"
                        on:click={() => selectPill(item.val_ID)}
                        aria-pressed={type === item.val_ID}
                    >
                        {item.text}
                    </button>
                {/each}
            </div>

            {#if showlainnyaInput}
                <div class="mt-3">
                    <label for="lainnyaType" class="input-label">
                        <input
                            type="text"
                            id="lainnyaType"
                            name="lainnyaType"
                            placeholder="Masukan Kategori Spesifik"
                            bind:value={lainnyaTypeInput}
                            required
                        >
                        <span class="required"></span>
                        <span class="line"></span>
                    </label>
                </div>
            {/if}
        </fieldset>

        <div class="col-span-1 sm:col-span-2">
            <div class="flex flex-col gap-2">
                <label for="pesan">Pesan</label>
                <textarea class="w-full min-h-[150px] p-3" name="pesan" id="pesan"></textarea>
            </div>
        </div>

        <div class="col-span-1 sm:col-span-2 flex justify-end">
            <button 
                type="submit" 
                class="bg-base text-white px-10 py-2 rounded-md btn-clip"
            >
                <span class="stroke"></span>
                <span class="text-sm tracking-wide">KIRIM</span>
            </button>
        </div>
    </form>
</section>

