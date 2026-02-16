<script>
    import "../app.css";
    import Navbar from '../routes/navbar/+page.svelte'
    import Background3D from '../components/Background3D.svelte';
</script>

<div class="relative w-full min-h-screen overflow-x-hidden">
    <Background3D />
    <Navbar />

    <main class="relative z-10 pt-24">
        <slot />
    </main>
</div>


<style></style>