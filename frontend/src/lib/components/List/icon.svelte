<script>
  export let name;
  export let size = 24;
  export let fill = 0;
  export let weight = 400;
  export let color = "#f85c3a;";
</script>

<span
  class="material-symbols-outlined"
  style="
      font-size: {size}px;
      color: {color};
      font-variation-settings:
         'FILL' {fill},
         'wght' {weight},
         'GRAD' 0,
         'opsz' {size};
   "
>
  {name}
</span>
