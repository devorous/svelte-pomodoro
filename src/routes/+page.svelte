<script lang="ts">
    import Timer from '$lib/components/Timer.svelte';
    import Controls from '$lib/components/Controls.svelte';
    let time = $state(1200);
    let type = $state('long');
    let count = 0;
    let interval: ReturnType<typeof setInterval> | undefined;
    

function start_timer() {
    clearInterval(interval);

    interval = setInterval(() => {
        time -= 1;

        if (time === 0) {
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
    }, 1000);
}

    function pause_timer(){
        clearInterval(interval);
    }

    function set_time(newTime: number){
        time = newTime;
    }
</script>


<Timer {time} {type} />
<Controls start={start_timer} pause={pause_timer} />



<style>
:global(body){
    box-sizing: border-box;
    box-shadow: inset 0 0 0 2px #3b3d3e; 
    background-color: #00000000;
    overflow:hidden;
    margin: 0; /* Ensure no default margin pushes the bar */
}
</style>

