<script>
    import { swipe } from "svelte-gestures";

    function handlerSwipe(event) {
        move(event.detail.direction);
    }

    function move(direction) {
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

<div>Click or swipe to move the preview image.</div>
<div
    class="swiper"
    use:swipe={{ timeframe: 300, minSwipeDistance: 100 }}
    on:swipe={handlerSwipe}
    style=""
>
    <table>
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <tr><td colspan="3" on:click={() => move("top")}>&uarr;</td></tr>
        <tr>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <td on:click={() => move("left")}>&larr;</td>
            <td />
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <td on:click={() => move("right")}>&rarr;</td>
        </tr><tr>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <td colspan="3" on:click={() => move("bottom")}>&darr;</td>
        </tr>
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
    div.swiper {
        width: 100%;
        height: 300px;
        justify-content: center;
        align-items: center;
        box-sizing: border-box;
        border: 1px solid black;
    }
    @media screen and (min-width: 1024px) {
        div.swiper {
            width: 300px;
        }
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
        user-select: none;
        cursor: pointer;
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
