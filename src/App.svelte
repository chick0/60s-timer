<script>
    import { onMount } from "svelte"
    import Timer from "./lib/Timer.svelte"
    import Controller from "./lib/Controller.svelte"

    onMount(() => {
        if ("wakeLock" in navigator) {
            navigator.wakeLock
                // @ts-ignore
                .request("screen")
                .then((lock) => {
                    console.log(lock)

                    lock.addEventListener("release", () => {
                        alert("Screen Wake Lock released:" + lock.released)
                    })
                })
                .catch((err) => {
                    console.error(`${err.name}, ${err.message}`)
                })
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
    div {
        display: flex;
    }

    div > div {
        height: 100vh;
        width: 50vw;
        justify-content: center;
        align-items: center;
    }
</style>
