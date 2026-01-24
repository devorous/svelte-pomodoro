<script lang="ts">
    import { onMount } from  'svelte';
    import Timer from '$lib/components/Timer.svelte';
    import Controls from '$lib/components/Controls.svelte';
    let sound_effect = new Audio('../src/lib/assets/effect.mp3');
    let time = $state(1200);
    let type = $state('long');
    let isPaused = $state(false);
    let isFlashing = $state(false);
    let flashColour = $state('red');
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
                sound_effect.play();
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

<div class="app">
     <div
      class="flash-overlay"
      class:active={isFlashing}
      style:background-color={flashColour}
  ></div>
    <Timer {time} {type} />

    <p class:invisible={isPaused}>Paused</p>

    <Controls start={start_timer} pause={pause_timer} {isPaused} />
</div>




<style>
    :global(body){
        overflow:hidden;
    }
    p{
        color: red;
    }
    .invisible{
        visibility: hidden;
    }
     .flash-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      opacity: 0;
      z-index: 9999;
  }
   .flash-overlay.active {
      animation: flash-pulse 1s ease-in-out;
  }
  @keyframes flash-pulse {
      0% { opacity: 0.8; }
      25% { opacity: 0.2; }
      50% { opacity: 0.8; }
      75% { opacity: 0.2; }
      100% { opacity: 0; }
  }
</style>

