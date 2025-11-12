<script>
    import { onMount, onDestroy } from 'svelte';
    import maplibregl from 'maplibre-gl';
    import 'maplibre-gl/dist/maplibre-gl.css';

    export let MAPTILER_API_KEY;

    let mapContainer;
    let map;
    let searchQuery = '';
    let filteredUmkm = [];

    // --- UMKM Data (Dummy JSON) ---
    const UMKM_DATA = [
        {
            id: 1,
            name: "Bakso Sultan Surabaya",
            category: "Food & Beverage",
            description: "Delicious and authentic Indonesian meatball stall, famous for its giant portions.",
            coords: [112.745, -7.260],
            hashtag: "#BaksoSultanSby",
            menu: [{ item: "Bakso Biasa", price: 15000 }, { item: "Bakso Urat Jumbo", price: 25000 }],
            details_link: "/umkm/bakso-sultan"
        },
        {
            id: 2,
            name: "Kopi Nusantara Co.",
            category: "Café & Coffee Shop",
            description: "Cozy café specializing in local East Java coffee beans. Great for work or meetings.",
            coords: [112.730, -7.275],
            hashtag: "#KopiNusantaraSby",
            menu: [{ item: "Latte", price: 20000 }, { item: "Es Kopi Susu", price: 18000 }],
            details_link: "/umkm/kopi-nusantara"
        },
        {
            id: 3,
            name: "Kerajinan Batik Tulis",
            category: "Handicraft & Apparel",
            description: "Handmade traditional Batik clothing and accessories, supporting local artisans.",
            coords: [112.765, -7.250],
            hashtag: "#BatikSurabaya",
            menu: [{ item: "Syal Batik", price: 50000 }, { item: "Kemeja Batik", price: 150000 }],
            details_link: "/umkm/batik-tulis"
        }
    ];

    const MAP_STYLE_URL = `https://api.maptiler.com/maps/dataviz-dark/style.json?key=${MAPTILER_API_KEY}`;
    const SURABAYA_CENTER = [112.7525, -7.2575];

    function initializeMap() {
    if (!mapContainer || map) return;

    try {
        map = new maplibregl.Map({
            container: mapContainer,
            style: MAP_STYLE_URL,
            center: SURABAYA_CENTER,
            zoom: 13,
            pitch: 0,
            bearing: 0,
            antialias: true
        });

        map.on('load', () => {
            updateMarkers(UMKM_DATA);

            // 🔹 Add zoom scaling for markers
            map.on('zoom', () => {
                const zoom = map.getZoom();
                const scale = Math.pow(zoom / 13, 0.8); // adjust exponent for sensitivity
                document.querySelectorAll('.custom-umkm-marker-wrapper').forEach(marker => {
                    marker.style.transform = `scale(${scale})`;
                });
            });
        });

        map.addControl(new maplibregl.NavigationControl({ showCompass: false }), 'bottom-right');
    } catch (error) {
        console.error("MapLibre initialization failed:", error);
        if (!error.message.includes('_getUIString')) {
            throw error;
        }
    }
}


    // --- Marker and Popup Logic ---
    function updateMarkers(data) {
        if (!map) return;
        document.querySelectorAll('.maplibregl-marker').forEach(marker => marker.remove());

        data.forEach(umkm => {
            const markerElement = createCustomMarkerElement(umkm);

            const popupHTML = `
                <div class="p-2 font-sans text-gray-800">
                    <h3 class="font-bold text-lg mb-1">${umkm.name}</h3>
                    <p class="text-xs text-gray-500 mb-2">Category: ${umkm.category}</p>
                    <p class="mb-2">${umkm.description}</p>
                    <p class="text-blue-500 font-mono text-sm mb-3">${umkm.hashtag}</p>
                    <a href="${umkm.details_link}" class="bg-orange-500 text-white px-3 py-1 rounded text-sm hover:bg-orange-600 transition">See Details</a>
                </div>
            `;

            new maplibregl.Marker({
                element: markerElement,
                anchor: 'center',
                offset: [0, -15] // keeps red bubble tip stable on coordinate
            })
                .setLngLat(umkm.coords)
                .setPopup(new maplibregl.Popup({ offset: 25 }).setHTML(popupHTML))
                .addTo(map);
        });
    }

    function createCustomMarkerElement(umkm) {
        const el = document.createElement('div');
        el.className = 'custom-umkm-marker-wrapper';

        const createTag = (text, className) => {
            const tag = document.createElement('div');
            tag.className = `tag-badge ${className}`;
            tag.textContent = text;
            return tag;
        };

        const nameTag = createTag(umkm.name, 'bg-red-500 text-white font-bold text-base shadow-lg');
        const categoryTag = createTag(umkm.category, 'bg-yellow-500 text-gray-900 font-medium text-xs shadow-md');
        const hashtagText = umkm.hashtag ? umkm.hashtag : '#IKUMKM';
        const hashtagTag = createTag(hashtagText, 'bg-green-500 text-white font-medium text-xs shadow-md');

        el.appendChild(nameTag);
        el.appendChild(categoryTag);
        el.appendChild(hashtagTag);

        return el;
    }

    // --- Search filtering ---
    $: {
        if (searchQuery) {
            const query = searchQuery.toLowerCase();
            filteredUmkm = UMKM_DATA.filter(umkm =>
                umkm.name.toLowerCase().includes(query) ||
                umkm.category.toLowerCase().includes(query) ||
                umkm.hashtag.toLowerCase().includes(query)
            );
        } else {
            filteredUmkm = UMKM_DATA;
        }

        if (map) updateMarkers(filteredUmkm);
    }

    onMount(() => initializeMap());
    onDestroy(() => map && map.remove());
</script>

<!-- Map Container -->
<div class="relative w-screen h-screen">
    <div bind:this={mapContainer} class="w-full h-full" />

    <!-- Search Box -->
    <div class="absolute top-4 left-1/2 transform -translate-x-1/2 z-10 w-full max-w-lg">
        <input
            type="text"
            bind:value={searchQuery}
            placeholder="Search UMKM by name, category, or hashtag..."
            class="w-full p-4 rounded-xl shadow-2xl bg-gray-800 text-white placeholder-gray-400 border-2 border-orange-500 focus:outline-none focus:ring-2 focus:ring-orange-500 transition duration-150"
        />
    </div>
</div>

<style>
/* Marker Wrapper */
:global(.custom-umkm-marker-wrapper) {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  z-index: 10;
  height: 100px;
  pointer-events: auto;
}

/* Tag Base Style */
:global(.tag-badge) {
  padding: 6px 14px;
  border-radius: 9999px;
  white-space: nowrap;
  text-align: center;
  position: absolute;
  transition: all 0.2s ease;
  font-family: 'Inter', sans-serif;
  letter-spacing: 0.2px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.2);
}

/* Tag Positioning */
:global(.custom-umkm-marker-wrapper > :nth-child(1)) {
  bottom: 0px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 30;
  clip-path: polygon(0% 0%, 100% 0%, 100% 80%, 55% 80%, 50% 100%, 45% 80%, 0% 80%);
  padding-bottom: 15px;
}

:global(.custom-umkm-marker-wrapper > :nth-child(2)) {
  bottom: 45px;
  left: 50%;
  transform: translateX(-110%);
  z-index: 20;
}

:global(.custom-umkm-marker-wrapper > :nth-child(3)) {
  bottom: 65px;
  left: 50%;
  transform: translateX(10%);
  z-index: 25;
}

/* Hover Animation */
:global(.custom-umkm-marker-wrapper:hover .tag-badge) {
  transform: translateY(-2px) scale(1.05);
  filter: brightness(1.1);
}
</style>
