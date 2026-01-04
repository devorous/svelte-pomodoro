<script lang="ts">
    import { onMount } from  'svelte';
    import Timer from '$lib/components/Timer.svelte';
    import Controls from '$lib/components/Controls.svelte';
    let time = $state(5);
    let type = $state('long');
    let isPaused = $state(false);
    let isFlashing = $state(false);
    let flashColour = $state('red');
    let count = 0;
    let interval: ReturnType<typeof setInterval> | undefined;

    onMount(() => {
        $effect(() => {
            if (isFlashing) {
                document.body.classList.add('flash-active');
                document.body.style.backgroundColor = flashColour; 
            } else {
                document.body.classList.remove('flash-active');
                document.body.style.backgroundColor = '';
            }
        });
    });

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
                        flashColour = 'green';
                    } else {
                        set_time(300);
                        type = 'short';
                        flashColour = 'green';
                    }
                } else {
                    set_time(1200);
                    type = 'long';
                    flashColour = 'red';
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


<Timer {time} {type} />

<p class:invisible={isPaused}>Paused</p>

<Controls start={start_timer} pause={pause_timer} {isPaused} />



<style>
    :global(body.flash-active) {
        animation: flash 1s ease-in-out;
    }
    @keyframes flash {
        0% { opacity: 1; }
        25% { opacity: 0.3; }
        50% { opacity: 1; }
        75% { opacity: 0.3; }
        100% { opacity: 1; }
    }
    p{
        color: red;
    }
    .invisible{
        visibility: hidden;
    }
</style>

