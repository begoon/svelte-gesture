<script>
    import { swipe } from "svelte-gestures";

    function handlerSwipe(event) {
        move(event.detail.direction);
    }

    function move(direction) {
        switch (direction) {
            case "left":
                setTimeout(() => {
                    if (--cx < 0) cx = mx - 1;
                    left = false;
                }, 1000);
                left = true;
                break;
            case "right":
                setTimeout(() => {
                    if (++cx >= mx) cx = 0;
                    right = false;
                }, 1000);
                right = true;
                break;
            case "top":
                setTimeout(() => {
                    if (--cy < 0) cy = my - 1;
                    down = false;
                }, 1000);
                down = true;
                break;
            case "bottom":
                setTimeout(() => {
                    if (++cy >= my) cy = 0;
                    up = false;
                }, 1000);
                up = true;
                break;
        }
    }

    import { files } from "./files.js";

    const images = files();

    const mx = images[0].length;
    let cx = Math.floor(mx / 2);

    const my = images.length;
    let cy = Math.floor(my / 2);

    let up = null,
        down = null,
        left = null,
        right = null;
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

<div
    class="preview"
    use:swipe={{ timeframe: 300, minSwipeDistance: 50 }}
    on:swipe={handlerSwipe}
>
    <!-- svelte-ignore a11y-missing-attribute -->
    <img
        draggable="false"
        src={images[cy][cx]}
        class="preview"
        class:preview-up={up}
        class:preview-down={down}
        class:preview-left={left}
        class:preview-right={right}
    />
</div>

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
    .preview {
        position: relative;
        height: 200px;
        width: 200px;
        overflow: hidden;
    }
    img.preview {
        position: absolute;
        border: 1px solid black;
        box-sizing: border-box;
    }
    img.preview-up {
        animation: up 1s ease-in-out;
    }
    img.preview-down {
        animation: down 1s ease-in-out;
    }
    img.preview-left {
        animation: left 1s ease-in-out;
    }
    img.preview-right {
        animation: right 1s ease-in-out;
    }
    img.icon {
        display: inline-block;
        width: 30px;
        height: auto;
        margin: 2px;
    }
    @keyframes up {
        0% {
            margin-top: 0;
        }
        100% {
            margin-top: 200px;
        }
    }
    @keyframes down {
        0% {
            margin-top: 0;
        }
        100% {
            margin-top: -200px;
        }
    }
    @keyframes left {
        0% {
            margin-left: 0;
        }
        100% {
            margin-left: -200px;
        }
    }
    @keyframes right {
        0% {
            margin-left: 0;
        }
        100% {
            margin-left: 200px;
        }
    }
    img.current {
        outline: 2px solid red;
    }
</style>
