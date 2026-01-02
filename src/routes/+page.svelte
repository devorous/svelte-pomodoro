<script lang="ts">
    import Timer from '$lib/components/Timer.svelte';
    import Controls from '$lib/components/Controls.svelte';
    let time = $state(1200);
    let type = $state('long');
    let isPaused = $state(false);
    let isFlashing = $state(false);
    let count = 0;
    let interval: ReturnType<typeof setInterval> | undefined;

    function triggerFlash() {
        isFlashing = true;
        setTimeout(() => { isFlashing = false; }, 1000); 
    }

    function start_timer() {
        clearInterval(interval);
        isPaused = !isPaused;
        interval = setInterval(() => {
            
            time -= 1;

            if (time === 0) {
                triggerFlash();
                if (type === 'long') {
                    count += 1;

                    if (count % 4 === 0) {
                        set_time(900);
                        type = 'medium';
                    } else {
                        set_time(300);
                        type = 'short';
                    }
                } else {
                    set_time(1200);
                    type = 'long';
                }
            }
        },1000);
    }

    function pause_timer(){
        clearInterval(interval);
        isPaused = !isPaused;
    }

    function set_time(newTime: number){
        time = newTime;
    }
</script>

<svelte:body class:flash-active={isFlashing} />

<Timer {time} {type} />

<p class:invisible={isPaused}>Paused</p>

<Controls start={start_timer} pause={pause_timer} {isPaused} />



<style>
    @keyframes flash{
        0% {background-color: #3b3d3e}
        5% {background-color: red}
        10% {background-color: #3b3d3e}
        75% {background-color: #3b3d3e}
        80% {background-color: red}
        85% {background-color: #3b3d3e}
        100% {background-color: #3b3d3e}
    }
    :global(body.flash-active){
        animation: flash 1s ease-in-out;
    }
    :global(body){
        box-sizing: border-box;
        box-shadow: inset 0 0 0 2px #3b3d3e; 
        background-color: #00000000;
        overflow:hidden;
    }
    p{
        color: red;
    }
    .invisible{
        visibility: hidden;
    }
</style>

