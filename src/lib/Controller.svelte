<script>
    import { onDestroy } from "svelte"
    import { timeLeft } from "../store"
    import TimerAudio from "../assets/timer.mp3"

    /**
     * 전체화면으로 전환
     */
    function requestFullscreen() {
        const element = document.documentElement

        // 브라우저별 전체화면으로 전환하는 함수가 달라 다음과 같이 처리함

        if (element.requestFullscreen) element.requestFullscreen()
        // @ts-ignore
        else if (element.webkitRequestFullscreen) element.webkitRequestFullscreen()
        // @ts-ignore
        else if (element.mozRequestFullScreen) element.mozRequestFullScreen()
        // @ts-ignore
        else if (element.msRequestFullscreen) element.msRequestFullscreen()
    }

    /**
     * 타이머 등록
     */
    function register() {
        requestFullscreen()

        intervalId = setInterval(() => {
            if ($timeLeft > 0) {
                timeLeft.set($timeLeft - 1)
            }

            if ($timeLeft == 0 && timerAudioController?.paused) {
                if (timerAudioController != undefined) {
                    timerAudioController.currentTime = 0
                }

                timerAudioController?.play()
                    .catch(() => {
                        alert("제한 시간이 종료되었습니다!")

                        if(confirm("소리를 재생하시겠습니까?")) {
                            timerAudioController?.play()
                                .catch((err) => {
                                    if(err.name == "NotAllowedError") {
                                        alert("알림 소리를 자동으로 재생할 수 없습니다! 아래 소리 재생 버튼을 눌러주세요")
                                        showPlayRequest = true
                                    } else {
                                        alert("알림 소리 재생에 오류가 발생했습니다.\n" + err)
                                    }
                                })
                        }
                    })
            }
        }, 1000)
    }

    /**
     * 타이머 등록 해제
     */
    function unregister() {
        if (intervalId != undefined) {
            clearInterval(intervalId)
            intervalId = undefined
            timerAudioController?.pause();
        }
    }

    /**
     * 타이머 초기화
     */
    function reset() {
        timeLeft.set(60)
        timerAudioController?.pause();
    }

    let intervalId = undefined

    /** @type {HTMLAudioElement} */
    let timerAudioController = undefined

    let showPlayRequest = false

    onDestroy(() => {
        unregister()
    })
</script>

<audio src="{TimerAudio}" bind:this="{timerAudioController}"></audio>

{#if showPlayRequest}
    <div>
        <button on:click="{() => {
            timerAudioController?.play()
                .then(() => {
                    showPlayRequest = false
                })
        }}">소리 재생</button>
    </div>
{/if}

<div>
    {#if intervalId == undefined}
        <button class="green" on:click="{register}">시작</button>
    {:else}
        <button class="red" on:click="{unregister}">일시정지</button>
    {/if}

    <button on:click="{reset}">초기화</button>
</div>

<style>
    div {
        width: 100%;
        text-align: center;
    }

    div button {
        display: block;
        width: 350px;
        margin: 10px;
        border: 0;
        border-radius: 0.25em;
        margin-left: auto;
        margin-right: auto;
        padding: 15px;
        font-size: 45px;
        color: #ffffff;
    }

    /* Mobile */
    @media (max-width: 768px) {
        div {
            display: flex;
        }

        button {
            width: calc(50% - 25px) !important;
            font-size: 20px !important;
        }
    }

    @media (prefers-color-scheme: dark) {
        button {
            background-color: #4f4f7d;
        }
    }

    @media (prefers-color-scheme: light) {
        button {
            background-color: #212121;
        }
    }

    button.green {
        background-color: #008000;
    }

    button.red {
        background-color: #dc143c;
    }
</style>
