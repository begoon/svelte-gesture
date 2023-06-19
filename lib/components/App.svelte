<script>
    import { swipe } from "svelte-gestures";

    function handlerSwipe(event) {
        move(event.detail.direction);
    }

    const swiping = {
        left: false,
        right: false,
        top: false,
        bottom: false,
    };

    function move(direction) {
        function bounce(f) {
            swiping[direction] = true;
            setTimeout(() => {
                f();
                setTimeout(() => {
                    swiping[direction] = false;
                }, 100);
            }, 1000);
        }
        const directions = {
            left: () => {
                if (--cx < 0) cx = mx - 1;
            },
            right: () => {
                if (++cx >= mx) cx = 0;
            },
            top: () => {
                if (--cy < 0) cy = my - 1;
            },
            bottom: () => {
                if (++cy >= my) cy = 0;
            },
        };
        bounce(directions[direction]);
    }

    import Arrow from "./Arrow.svelte";
    import { files } from "./files.js";

    const images = files();

    const mx = images[0].length;
    let cx = Math.floor(mx / 2);

    const my = images.length;
    let cy = Math.floor(my / 2);
</script>

<div>Use arrows or swipe to switch the image.</div>
<div
    class="swiper"
    use:swipe={{ timeframe: 300, minSwipeDistance: 50 }}
    on:swipe={handlerSwipe}
>
    <table>
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <tr><td colspan="3"><Arrow direction="top" {move} /></td></tr>
        <tr>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <td><Arrow direction="left" {move} /></td>
            <td class="preview">
                <div class="preview">
                    <!-- svelte-ignore a11y-missing-attribute -->
                    <img
                        draggable="false"
                        src={images[cy][cx]}
                        class="preview"
                        class:preview-bottom={swiping.bottom}
                        class:preview-top={swiping.top}
                        class:preview-left={swiping.left}
                        class:preview-right={swiping.right}
                    />
                </div>
            </td>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <td><Arrow direction="right" {move} /></td>
        </tr>
        <tr><td colspan="3"><Arrow direction="bottom" {move} /></td></tr>
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

<style>
    * {
        margin: 0;
        padding: 0;
        --swiper-size: 200px;
    }
    div.swiper {
        width: 100%;
        aspect-ratio: 1;
        justify-content: center;
        align-items: center;
        box-sizing: bordaer-box;
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
        font-size: 40px;
        user-select: none;
        cursor: pointer;
    }
    td .preview {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
    }
    .preview {
        position: relative;
        height: var(--swiper-size);
        width: var(--swiper-size);
        overflow: hidden;
    }
    img.preview {
        position: absolute;
        top: 0;
        left: 0;
        border: 1px solid black;
        box-sizing: border-box;
    }
    img.preview-bottom {
        animation: bottom 1s ease-in-out forwards;
        opacity: 0.5;
    }
    img.preview-top {
        animation: top 1s ease-in-out forwards;
    }
    img.preview-left {
        animation: left 1s ease-in-out forwards;
    }
    img.preview-right {
        animation: right 1s ease-in-out forwards;
    }
    img.icon {
        display: inline-block;
        width: 30px;
        height: auto;
        margin: 2px;
    }
    @keyframes bottom {
        0% {
            margin-top: 0;
        }
        100% {
            margin-top: var(--swiper-size);
        }
    }
    @keyframes top {
        0% {
            margin-top: 0;
        }
        100% {
            margin-top: calc(-1 * var(--swiper-size));
        }
    }
    @keyframes left {
        0% {
            margin-left: 0;
        }
        100% {
            margin-left: calc(-1 * var(--swiper-size));
        }
    }
    @keyframes right {
        0% {
            margin-left: 0;
        }
        100% {
            margin-left: var(--swiper-size);
        }
    }
    img.current {
        outline: 2px solid red;
    }
</style>
