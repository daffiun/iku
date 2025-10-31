<script>
    import { onMount, onDestroy } from 'svelte';
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

    import Logo from "$lib/assets/images/logos/logo.svg"; 

    const navLinks = [
        { text: 'Tentang Kami' },
        { text: 'Fitur', active: true },
        { text: 'Tim' },
        { text: 'FAQ' },
        { text: 'Kontak' },
    ];

    let textLogo;
    let navContainer;
    let items;

    onMount(() => {
    gsap.registerPlugin(ScrollTrigger);
    const items = gsap.utils.toArray('.text-nav');

    gsap.context(() => {
        let lastScroll = 0;
        let currentState = null;
        let lastDirection = 0;

        ScrollTrigger.create({
        trigger: 'body',
        start: 'top top',
        end: 'bottom bottom',
        onUpdate: (self) => {
            const scrollDirection = self.direction; // 1 = down, -1 = up
            const scrollSm = Math.abs(self.scroll() - lastScroll);

            if (scrollSm < 15) return;
            lastScroll = self.scroll();

            if (scrollDirection !== lastDirection) {
                if (currentState) {
                    currentState.kill();
                }
            } else if (currentState && currentState.isActive()) {
                return; 
            }
            lastDirection = scrollDirection;

            // Scrol down
            if (scrollDirection === 1) {
            currentState = gsap.timeline({ defaults: { ease: 'power3.out' } })
                .to(items, {
                opacity: 0,
                yPercent: -20,
                duration: 0.4,
                stagger: 0.06,
                overwrite: 'auto'
                })
                .to(navContainer, {
                width: 0,
                duration: 0.6,
                borderRight: 'none',
                marginRight: 0,
                paddingRight: 0,
                }, "-=0.2");
            }

            // Scroll up
            if (scrollDirection === -1) {
            currentState = gsap.timeline({ defaults: { ease: 'power3.out' } })
                .to(navContainer, {
                    width: 'auto',
                    duration: 0.6,
                    borderRight: '',
                    marginRight: '',
                    paddingRight: '',
                }, 0)

                .to(items, {
                opacity: 1,
                yPercent: 0,
                duration: 0.4,
                stagger: { each: 0.06, from: 'end' },
                overwrite: 'auto',
                }, 0)
            }
        },
        });
    });
    });

    onDestroy(() => {
    ScrollTrigger.getAll().forEach((t) => t.kill());
    });

</script>

<header class="sticky top-0 z-50 duration-300 ease-out py-4 px-6">
    <div class="max-w-5xl mx-auto flex-center gap-3">
        <div class="flex-between h-13 py-3 px-5 rounded-xl transition-all duration-300 bg-dark">
            <div class="flex items-center space-x-4 overflow-hidden">
                <a href="/" class="flex items-center space-x-2">
                    <img src="{Logo}" alt="IKU Logo" class="h-15 w-15 text-orange-500" />
                    <span bind:this={textLogo} class="text-sm text-white sm:inline whitespace-nowrap">Index Kewirausahaan UMKM</span>
                </a>
            </div>

            <nav bind:this={navContainer} class="flex items-center space-x-6 ml-6 pl-6 mr-3 pr-3 border-x-gray">
                <ul class="flex-center space-x-6 text-white text-sm font-medium">
                    {#each navLinks as link}
                        <li class="group relative nav-link">
                            <a href="#" class="nav-slide-link block h-6 overflow-hidden 
                                            transition-colors duration-300">
                                
                                <div class="nav-slide-wrapper flex flex-col transform transition-transform duration-300 {link.active ? 'text-orange-500' : 'text-gray-300'}">
                                    <span class="block h-6 shrink-0 text-nav">{link.text}</span>
                                    <span class="block h-6 shrink-0" aria-hidden="true">{link.text}</span>
                                </div>
                            </a>
                        </li>
                    {/each}
                </ul>
            </nav>

            <a href="#" class="bg-transparant relative border border-white text-white hover:border-base hover:bg-base btn-nav py-1 px-3 btn-clip">
                <span class="stroke"></span>
                <span class="text-sm text-white whitespace-nowrap">GABUNG</span>
            </a>
        </div>
        <div>
            <a href="#contact" 
                class="relative flex-center h-13 rounded-xl px-4 py-2 w-[166px] text-sm nav-link overflow-hidden group bg-dark text-white">
                
                <span class="font-bold relative flex-center z-20 transition-transform duration-300 ease-out 
                            group-hover:translate-x-[-50%] -translate-x-4 group-hover:opacity-0">
                    Jelajahi Peta
                </span>

                <div class="absolute right-3 text-black bg-base rounded-md z-20 
                            transition-all duration-300 ease-out 
                            group-hover:w-full group-hover:rounded-xl group-hover:right-0 group-hover:h-full h-8 w-8">
                    
                    <span class="abs-center transition-transform duration-300 ease-out">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 12 12" height="20px" width="20px" fill="none" class="c-custom-svg">
                            <path d="M1 5.25C0.585786 5.25 0.25 5.58579 0.25 6C0.25 6.41421 0.585786 6.75 1 6.75V5.25ZM11.5303 6.53033C11.8232 6.23744 11.8232 5.76256 11.5303 5.46967L6.75736 0.696699C6.46447 0.403806 5.98959 0.403806 5.6967 0.696699C5.40381 0.989593 5.40381 1.46447 5.6967 1.75736L9.93934 6L5.6967 10.2426C5.40381 10.5355 5.40381 11.0104 5.6967 11.3033C5.98959 11.5962 6.46447 11.5962 6.75736 11.3033L11.5303 6.53033ZM1 6V6.75H11V6V5.25H1V6Z" fill="currentColor"></path>
                        </svg>
                    </span>
                </div>
            </a>
        </div>

                    
    </div>
</header>
