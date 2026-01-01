<script lang="ts">
    let { time = 1200, type = 'long' } = $props();

</script>


<div data-type={type}>
    <p>{(time-time%60)/60}:{time%60 < 10 ? '0'+time%60 : time%60}</p>
</div>

<style>
    div{
        user-select: none;
        font-size:clamp(3rem, 20vmin, 6rem);
    }
    [data-type="short"] {
        color: green;
    }
    [data-type="medium"]{
        color:orangered;
    }
</style>