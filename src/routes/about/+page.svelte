<script>
// @ts-nocheck

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


<div class="px-4 pt-5 text-white w-full pb-10">

    <div class="md:flex grid md:grid-cols-2 ">
        <div class="w-full ">
            <div class="flex h-full">
                <div class="text-center m-auto md:block flex flex-col">
                    <div class="font-semibold text-[60px] text-center hidden md:block">
                        About
                    </div>

                    <div class="p-4 animate-fade-right animate-once animate-delay-300">
                        <div class="text-[36px] font-semibold">So, Who am I?</div>
                        <div class="text-[16px] md:px-10">
                            As a web developer and designer who lives for the freedom of the online world, I extend that
                            freedom onto the roads, seeking new inspirations and perspectives on my trusty motorcycle.
                        </div>
                    </div>

                    <div class="flex m-auto md:hidden items-center gap-4 py-4 animate-bounce animate-twice animate-delay-300">
                        <button on:click={()=>gotoSocialMedia('linkedIn')} class=" transition duration-500 hover:scale-125 ease-in-out ">
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
                </div>
            </div>
        </div>

        <div class="w-full flex animate-fade-left animate-once animate-delay-300">
            <img src="/working.png" class="m-auto max-h-[90vh]" alt="">
        </div>
    
    </div>

    <div class="md:flex grid grid-cols-2">

        <div class="w-full hidden">

            <div class="font-semibold text-[26px] text-center">
                Contact
            </div>
            <div>
                <div class="text-center">
                    <div class="p-4">
                        <h1>So, who am I?</h1>
                        <h5>
                            As a web developer and designer who lives for the freedom of the online world, I extend that
                            freedom onto the roads, seeking new inspirations and perspectives on my trusty motorcycle.
                        </h5>
                    </div>
                </div>
            </div>
        </div>

        
    </div>
</div>