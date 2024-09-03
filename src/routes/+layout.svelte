<script>
    import "../app.css";
    import Navbar from '../routes/navbar/+page.svelte'
</script>

<div class="w-full h-full min-h-[100vh] bg-stone-950">
    <Navbar></Navbar>

    <slot></slot>
    
</div>

<style></style>