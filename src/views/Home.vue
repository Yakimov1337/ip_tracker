<template>
    <div class="flex flex-col h-screen max-h-screen">
        <!-- Search / Results -->
        <div class="flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32">
            <!-- Search Input    -->
            <div class="w-full mx-w-screen-sm" >
                <h1 class="text-white text-center text-3xl pb-4">IP Address Tracker</h1>
                <div class="flex">
                    <input class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none" 
                    type="text"
                    placeholder="Search for any IP address or leave empty to get your ip info"
                    />

                 
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:"Home",
    components: {

    },
};
</script>