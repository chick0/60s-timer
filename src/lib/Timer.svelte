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

<p bind:this="{timer}">{$timeLeft}</p>

<style>
    p {
        font-family: Chivo Mono;
        font-weight: 900;
        font-size: 300px;
        width: 100%;
        text-align: center;
    }

    @media (min-width: 1200px) {
        p {
            font-size: 500px;
        }
    }
</style>
