<script>
    import { pan, swipe } from "svelte-gestures";
    let x;
    let y;
    let target;

    function handlerPan(event) {
        x = event.detail.x;
        y = event.detail.y;
        target = event.detail.target;
    }

    let direction;
    function handlerSwipe(event) {
        direction = event.detail.direction;
    }
</script>

Pan
<div use:pan={{ delay: 300 }} on:pan={handlerPan}>
    pan: {x}
    {y}
</div>

Swipe
<div
    use:swipe={{ timeframe: 300, minSwipeDistance: 100, touchAction: "pan-y" }}
    on:swipe={handlerSwipe}
>
    This one <b>swipes only in horizontal directions</b>:
    <span style="color: red;">{direction}</span><br />
    You can scroll vertically as normal. It is due to
    <b>touchAction: 'pan-y'</b>
</div>

<div
    use:swipe={{ timeframe: 300, minSwipeDistance: 100 }}
    on:swipe={handlerSwipe}
>
    This <b>swipes in all directions</b>:
    <span style="color:red;">{direction}</span>
</div>

<style>
    div {
        width: 500px;
        height: 500px;
        border: 1px solid black;
    }
</style>
