<script>
    import { swipe } from "svelte-gestures";
    let direction;
    function handlerSwipe(event) {
        direction = event.detail.direction;
        switch (direction) {
            case "left":
                if (--cx < 0) cx = mx - 1;
                break;
            case "right":
                if (++cx >= mx) cx = 0;
                break;
            case "top":
                if (--cy < 0) cy = my - 1;
                break;
            case "bottom":
                if (++cy >= my) cy = 0;
                break;
        }
    }

    import { files } from "./files.js";

    const images = files();

    const mx = images[0].length;
    let cx = Math.floor(mx / 2);

    const my = images.length;
    let cy = Math.floor(my / 2);
</script>

<div class="action">
    {#if direction}
        Действие:
        <span style="color: red;">{direction}</span>
    {/if}
</div>

<div
    class="swiper"
    use:swipe={{ timeframe: 300, minSwipeDistance: 100 }}
    on:swipe={handlerSwipe}
    style="justify-content: center; align-items: center;"
>
    <table>
        <tr><td colspan="3">&uarr;</td></tr>
        <tr>
            <td>&larr;</td>
            <td />
            <td>&rarr;</td>
        </tr><tr><td colspan="3">&darr;</td></tr>
    </table>
</div>

{#each images as names, y}
    <ul>
        {#each names as name, x}
            {@const current = x === cx && y === cy}
            <!-- svelte-ignore a11y-missing-attribute -->
            <img src={name} class="icon" class:current />
        {/each}
    </ul>
    <!-- svelte-ignore a11y-missing-attribute -->
{/each}
<!-- svelte-ignore a11y-missing-attribute -->
<img src={images[cy][cx]} class="preview" />

<style>
    * {
        margin: 0;
        padding: 0;
    }
    div.action {
        display: block;
        height: 1.5em;
    }
    div.swiper {
        width: 100%;
        height: 300px;
        box-sizing: border-box;
        border: 1px solid black;
    }
    table {
        width: 100%;
        height: 100%;
        border-collapse: collapse;
    }
    td {
        text-align: center;
        vertical-align: middle;
        font-size: 70px;
    }
    img.preview {
        height: 200px;
        width: auto;
        border: 1px solid black;
    }
    img.icon {
        display: inline-block;
        width: 30px;
        height: auto;
        margin: 2px;
    }
    img.current {
        outline: 2px solid red;
    }
</style>
