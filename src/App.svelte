<script>
    import { onMount } from "svelte"
    import Timer from "./lib/Timer.svelte"
    import Controller from "./lib/Controller.svelte"

    onMount(() => {
        if ("wakeLock" in navigator) {
            // @ts-ignore
            navigator.wakeLock.request("screen")
        } else {
            console.warn("화면 잠금 방지를 요청할 수 없음")
        }
    })
</script>

<div>
    <div>
        <Timer />
    </div>
    <div>
        <Controller />
    </div>
</div>

<style>
    /* Not Mobile */
    @media (min-width: 768px) {
        div {
            display: flex;
        }

        div > div {
            height: 100vh;
            width: 50vw;
            justify-content: center;
            align-items: center;
        }
    }
</style>
