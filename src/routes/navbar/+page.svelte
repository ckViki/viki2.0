<script>
// @ts-nocheck

    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { onMount } from "svelte";


    let pagepath = '';

    onMount(()=>{
        const path = page.subscribe((value)=>{
            pagepath = value.route?.id
        })
    })

    function gotoSocialMedia(socialmedia) {
        if(socialmedia == 'insta'){   
            window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'twitter') {
            // window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'whatsup') {
            window.open('https://wa.me/9629846371?text=Hi', '_blank');
        } else if (socialmedia == 'linkedIn') {
            window.open('https://www.linkedin.com/in/vignesh-kumar-c-02915b203', '_blank');
        }
    }

</script>
<div class="flex items-center">
    
    <div class="hidden md:flex items-center gap-1 md:gap-4 py-1 md:py-4">
        <button on:click={()=>gotoSocialMedia('linkedIn')} class="ml-2 transition duration-500 hover:scale-125 ease-in-out ">
            <img class="w-7 h-7" src="/socialmedia/linkedin.png" alt="">
        </button>

        <button on:click={()=>gotoSocialMedia('whatsup')} class="transition duration-500 hover:scale-125 ease-in-out ">
            <img class="w-7 h-7" src="/socialmedia/whatsup.png" alt="">
        </button>

        <button on:click={()=>gotoSocialMedia('insta')} class="transition duration-500 hover:scale-125 ease-in-out ">
            <img class="w-7 h-7" src="/socialmedia/insta.png" alt="">
        </button>

        <button class="transition duration-500 hover:scale-125 ease-in-out ">
            <img class="w-7 h-7" src="/socialmedia/telegram.png" alt="">
        </button>
    </div>

    <div class="flex items-center gap-4  md:gap-8 m-auto md:m-0 md:ml-auto md:mx-4 py-4">
        <button on:click={()=>goto('/')} class="{pagepath == '/' ? 'border-b-2 px-1 border-green-500' : ''} text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Home
        </button>
        
        <button on:click={()=>goto('/work')} class="{pagepath == '/work' ? 'border-b-2 px-1 border-green-500' : ''} text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Work
        </button>

        <button on:click={()=>goto('/skills')} class="{pagepath == '/skills' ? 'border-b-2 px-1 border-green-500' : ''} text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Skill
        </button>

        <button on:click={()=>goto('/about')}  class="{pagepath == '/about' ? 'border-b-2 px-1 border-green-500' : ''} text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            About
        </button>

    </div>
</div>