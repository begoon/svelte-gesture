<script>
    export let direction;
    export let move;

    const angle = {
        top: 0,
        right: 90,
        bottom: 180,
        left: 270,
    }[direction];
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div style="transform: rotate({angle}deg);" on:click={() => move(direction)}>
    ⇧
</div>
