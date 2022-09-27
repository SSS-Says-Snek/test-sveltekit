<script lang="ts">
    let num_clicks: number = 0;
</script>

<div class="main">
    <h1>Welcome to Snektodo!</h1>
    <h3>You have clicked the button {num_clicks} time{num_clicks === 1 ? '' : 's'}!</h3>
    <button on:click="{() => num_clicks++}">Click button</button>
</div>

<style>
    .main {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
</style>