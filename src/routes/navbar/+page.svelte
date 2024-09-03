<script>
// @ts-nocheck


    import { goto } from "$app/navigation";

    function gotoSocialMedia(socialmedia) {
        if(socialmedia == 'insta'){   
            window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'twitter') {
            window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'whatsup') {
            window.open('https://wa.me/9629846371', '_blank');
        } else if (socialmedia == 'linkedIn') {
            window.open('https://www.instagram.com/c_k_viki/', '_blank');
        }
    }

</script>
<div class="flex items-center">
    
    <div class="hidden md:flex items-center gap-1 md:gap-4 py-1 md:py-4">
        <button on:click={()=>gotoSocialMedia('linkedIn')} class="ml-2 transition duration-500 hover:scale-125 ease-in-out ">
            <img class="w-7 h-7" src="/socialmedia/linkedin.png" alt="">
        </button>

        <button class="transition duration-500 hover:scale-125 ease-in-out ">
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
        <button on:click={()=>goto('/')} class="text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Home
        </button>
        
        <button on:click={()=>goto('/work')} class="text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Work
        </button>

        <button on:click={()=>goto('/skills')} class="text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            Skill
        </button>

        <button on:click={()=>goto('/about')}  class="text-[16px] transition duration-500 hover:scale-125 ease-in-out text-white">
            About
        </button>

    </div>
</div>