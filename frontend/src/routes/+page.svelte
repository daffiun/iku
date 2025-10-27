<script>
  import { onMount } from 'svelte';
  import gsap from 'gsap';

  // This variable will hold a reference to the actual path element
  let pathElement;

  onMount(() => {
    // 1. Get the total length of the path.
    // This is crucial for the 'drawing' effect.
    const pathLength = pathElement.getTotalLength();

    // 2. Set the initial state for the animation.
    // The path should be invisible (dashoffset = pathLength) but ready to draw (dasharray = pathLength).
    // Note: Your SVG already has pathLength="1" stroke-dasharray="1px 1px",
    // which is used for relative path lengths. For GSAP animation, it's safer
    // to calculate the absolute length and use those values.

    gsap.set(pathElement, {
      strokeDasharray: pathLength,
      strokeDashoffset: pathLength
    });

    // 3. Animate the stroke-dashoffset to 0.
    // When the offset goes from pathLength to 0, the line appears.
    gsap.to(pathElement, {
      strokeDashoffset: 0,
      duration: 3, // How long the painting takes (e.g., 3 seconds)
      ease: 'power3.inOut' // A smooth easing function
    });
  });
</script>

<svg
  xmlns="http://www.w3.org/2000/svg"
  width="100%"
  height="100%"
  viewBox="0 0 500 351"
>
  <path
    d="M 486.864 13.61 C 390.545 194.927 248.175 159.17 204.087 125.947 C 158.968 91.947 218.97 66.259 287.488 106.239 C 356.006 146.219 355.756 187.164 337.472 207.033 C 288.765 259.964 159.719 192.438 143.713 210.412 C 138.097 216.718 190.328 248.139 220.094 267.002 C 157.192 262.78 57.785 292.624 13.136 337.39"
    stroke="rgb(146, 137, 198)"
    stroke-width="26.27"
    stroke-linejoin="round"
    stroke-linecap="round"
    fill="transparent"
    opacity="1"
    will-change="auto"
    
    bind:this={pathElement} />
</svg>
<div class="pb-96"></div>