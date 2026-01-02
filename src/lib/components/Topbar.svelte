<script lang="ts">
    let { title } = $props();
    import { getCurrentWindow } from '@tauri-apps/api/window';
    async function closeWindow() {
        if (window.__TAURI_INTERNALS__) {
            const window = getCurrentWindow();
            await window.close();
        } else {
            console.log("Tauri not available");
        }
    }

</script>

<div>
    <p>{title}</p>
    <a href="." onclick={closeWindow}>X</a>
</div>

<style>
    div{
        -webkit-app-region: drag;
        position:absolute;
        top:0;
        display: flex;
        justify-content: space-between;
        width:100%;
        height:2rem;
        background-color:#222;
        border-left: 2px solid #3b3d3e;
        border-right: 2px solid #3b3d3e;
    }
    p{
        line-height:0;
        color:#DDD;
        padding-left: 0.5rem;
        font-family: monospace;
        font-size:1rem;
        font-weight:600;
    }
    a{
        -webkit-app-region: no-drag;
        text-decoration: none;
        cursor: pointer;
        color:#DDD;
        line-height: 1.1;
        padding-right:0.6em;
        padding-left:0.8rem;
        font-size:1.5rem;
        font-family:'Segoe UI';
        font-weight:650;
        z-index: 1000;
        border: 2px solid #111;
        border-radius:3px;
        transform: translateX(-3px);
    }
    a:hover{
        background-color:#333;
        color: orange;
    }
</style>