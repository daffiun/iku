<script>
	import { onMount, onDestroy } from 'svelte';
	import maplibregl from 'maplibre-gl';
	import 'maplibre-gl/dist/maplibre-gl.css';

	export let MAPTILER_API_KEY;

	let map;
	let mapContainer;
	let loading = true;
	let searchQuery = "";
	let filteredUmkm = [];

	const categoryMap = {
		"1": "F&B",
		"2": "BB",
		"3": "ANF",
		"4": "JASA",
		"5": "LAINNYA"
	};

	const categoryIconMap = {
		"F&B": "restaurant",
		"BB": "storefront",
		"ANF": "checkroom",
		"JASA": "construction",
		"LAINNYA": "category"
	};

	import rawUmkm from '$lib/assets/dummy/umkm.json';

	const UMKM_DATA = rawUmkm.map(u => {
		const cat = categoryMap[u.category_id] || "LAINNYA";
		return {
			id: u.id,
			name: u.nama,
			category: cat,
			description: u.desc ?? "",
			image: u.image?.[0] ?? "",
			coords: [u.coor.lng, u.coor.lat],
			icon: categoryIconMap[cat],
			details_link: `/umkm/${u.id}`
		};
	});

	const MAP_STYLE_URL = `https://api.maptiler.com/maps/darkmatter/style.json?key=${MAPTILER_API_KEY}`;
	const CENTER = [112.7525, -7.2575];

	onMount(() => {
		map = new maplibregl.Map({
			container: mapContainer,
			style: MAP_STYLE_URL,
			center: CENTER,
			zoom: 13,
			antialias: true
		});

		map.addControl(new maplibregl.NavigationControl({ showCompass: false }), "bottom-right");

		map.on("load", () => {
			updateMarkers(UMKM_DATA);
			loading = false;
		});

		map.on("zoom", updateZoomState);
	});

	onDestroy(() => map && map.remove());

	function updateZoomState() {
		const zoom = map.getZoom();
		const showName = zoom >= 13;
		document.querySelectorAll(".pin-text").forEach(t => {
			t.style.display = showName ? "inline" : "none";
		});
	}

	function updateMarkers(list) {
		document.querySelectorAll(".maplibregl-marker").forEach(m => m.remove());

		list.forEach(umkm => {
			const el = createMarker(umkm);

			const popup = new maplibregl.Popup({ offset: 25 }).setHTML(`
                <div class="p-2 font-sans text-gray-800">
					<img src="${umkm.image}" class="w-full h-32 object-cover rounded mb-2"/>
                    <h3 class="font-bold text-lg mb-1">${umkm.name}</h3>
                    <p class="mb-3">${umkm.description}</p>
                    <a href="${umkm.details_link}"
                        class="bg-orange-500 text-white px-3 py-1 rounded text-sm hover:bg-orange-600 transition">
                        See Details
                    </a>
                </div>
            `);

			new maplibregl.Marker({ element: el, anchor: "bottom" })
				.setLngLat(umkm.coords)
				.setPopup(popup)
				.addTo(map);
		});

		updateZoomState();
	}

	function createMarker(umkm) {
		const wrapper = document.createElement("div");
		wrapper.className = "pin-root";

		const bubble = document.createElement("div");
		bubble.className = "pin-bubble";

		const icon = document.createElement("span");
		icon.className = "material-symbols-outlined pin-icon";
		icon.textContent = umkm.icon;

		const text = document.createElement("span");
		text.className = "pin-text";
		text.textContent = umkm.name;

		bubble.appendChild(icon);
		bubble.appendChild(text);

		const pointer = document.createElement("div");
		pointer.className = "pin-pointer";

		wrapper.appendChild(bubble);
		wrapper.appendChild(pointer);

		return wrapper;
	}

	$: {
		if (!searchQuery) filteredUmkm = UMKM_DATA;
		else {
			const q = searchQuery.toLowerCase();
			filteredUmkm = UMKM_DATA.filter(u =>
				u.name.toLowerCase().includes(q)
			);
		}
		if (map) updateMarkers(filteredUmkm);
	}
</script>

<div class="relative w-screen h-screen">
	{#if loading}
	<div class="absolute inset-0 flex items-center justify-center bg-black/70 z-20">
		<div class="flex flex-col items-center">
			<div class="loader mb-3"></div>
			<p class="text-white text-lg">Loading map...</p>
		</div>
	</div>
	{/if}

	<div bind:this={mapContainer} class="w-full h-full"></div>

	<div class="absolute top-24 left-1/2 -translate-x-1/2 z-10 w-full max-w-lg">
		<input
			type="text"
			bind:value={searchQuery}
			placeholder="Search UMKM..."
			class="w-full p-4 rounded-xl shadow-2xl bg-gray-800 text-white placeholder-gray-400 border-2 border-orange-500 focus:ring-2 focus:ring-orange-500"
		/>
	</div>
</div>

<style>
:global(.material-symbols-outlined) {
  font-variation-settings: 'FILL' 0, 'wght' 500, 'GRAD' 0, 'opsz' 48;
}

:global(.pin-root) {
  display: flex;
  flex-direction: column;
  align-items: center;
}

:global(.pin-bubble) {
  display: flex;
  align-items: center;
  gap: 6px;
  background: linear-gradient(135deg, #ff8746, #ff4d00);
  padding: 6px 12px;
  border-radius: 20px;
  color: white;
  font-size: 13px;
  white-space: nowrap;
  box-shadow: 0 3px 12px rgba(0,0,0,0.4);
}

:global(.pin-icon) {
  font-size: 20px;
}

:global(.pin-text) {
  font-weight: 600;
}

:global(.pin-pointer) {
  width: 14px;
  height: 14px;
  background: #ff4d00;
  clip-path: polygon(50% 100%, 0 0, 100% 0);
  margin-top: -2px;
  box-shadow: 0 3px 12px rgba(0,0,0,0.4);
}

.loader {
  width: 30px;
  height: 30px;
  border: 4px solid #ffffff33;
  border-top-color: #ff7a18;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin { to { transform: rotate(360deg); } }
</style>
