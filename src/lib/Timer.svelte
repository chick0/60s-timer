<script>
    import { onDestroy, onMount } from "svelte"
    import { timeLeft } from "../store"

    let timer = undefined
    let intervalId = undefined

    onMount(() => {
        intervalId = setInterval(() => {
            if ($timeLeft < 15) {
                if (timer.dataset.status != "a") {
                    timer.dataset.status = "a"
                    timer.style = "color: #dc143c"
                } else {
                    timer.dataset.status = "b"
                    timer.style = "color: #ff5151"
                }
            } else {
                timer.style = ""
            }
        }, 250)
    })

    onDestroy(() => {
        clearInterval(intervalId)
        intervalId = undefined
    })
</script>

<p class="timer shadow" bind:this="{timer}">{$timeLeft}</p>

<style>
    p {
        font-size: 300px;
        font-weight: bold;
        width: 100%;
        text-align: center;
    }

    @media (min-width: 1000px) {
        p {
            font-size: 500px;
        }
    }
</style>
