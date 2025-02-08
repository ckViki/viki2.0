<script>
    import "../app.css";
    import Navbar from '../routes/navbar/+page.svelte'
</script>

<div class="w-full h-full min-h-[100vh] bg-black">
    <Navbar></Navbar>

    <slot></slot>
    
</div>

<style></style>