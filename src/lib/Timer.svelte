<script>
    import { onDestroy, onMount } from "svelte"
    import { timeLeft } from "../store"

    let timer = undefined
    let intervalId = undefined

    onMount(() => {
        intervalId = setInterval(() => {
            if ($timeLeft < 15) {
                timer.classList.toggle("warn")
            } else {
                timer.classList.remove("warn")
            }
        }, 500)
    })

    onDestroy(() => {
        clearInterval(intervalId)
        intervalId = undefined
    })
</script>

<p class="timer warn" bind:this="{timer}">{$timeLeft}</p>

<style>
    p {
        font-size: 300px;
        font-weight: bold;
        width: 100%;
        text-align: center;
    }

    .warn {
        color: crimson;
        text-shadow: crimson 1px 0 15px;
    }
</style>
