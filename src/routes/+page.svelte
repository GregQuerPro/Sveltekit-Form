<script>
    import { supabase } from "$lib/supabaseClient.js";
    import Bed from "../lib/Bed.svelte";
    import Dressing from "../lib/Dressing.svelte";
    import Tv from "../lib/Tv.svelte";

    async function fetchAllProperties() {
        const { data, error } = await supabase.from("property").select(`
            agencement->floors
        `);

        if (error) throw new Error(error.message);
        return data;
    }

    function handleFloorCtnClick(e) {
        console.log(e);
    }
</script>

<h1>Edition Maison</h1>
{#await fetchAllProperties()}
    <p>Chargement Maison...</p>
{:then data}
    {#each data as property}
        <div class="house-ctn">
            {#each property.floors as floor}
                <div class="house-btn" on:click={handleFloorCtnClick}>
                    <div class="house-btn-item">{floor.name}</div>
                    <div class="floor-ctn">
                        {#each floor.rooms as room}
                            <div class="floor-btn">
                                <div class="floor-item">{room.name}</div>
                                <!-- <div class="item item-furnitures">
                                    {#each room.furnitures as furniture}
                                        <div class="sub-item">
                                            <div>{furniture.name}</div>
                                            {#if furniture.type === "bed"}
                                                <Bed bed={furniture} />
                                            {:else if furniture.type === "dressing"}
                                                <Dressing dressing={furniture} />
                                            {:else if furniture.type === "tv"}
                                                <Tv tv={furniture} />
                                            {/if}
                                        </div>
                                    {/each}
                                </div> -->
                            </div>
                        {/each}
                    </div>
                </div>
            {/each}
        </div>
    {/each}
{:catch error}
    <p>Something went wrong while fetching the data:</p>
    <pre>{error}</pre>
{/await}

<style>
    /* House Level */

    .house-ctn {
        padding: 15px;
        border-radius: 5px;
        background: lightgray;
    }

    .house-btn {
        display: flex;
        /* display: none; */
        padding: 15px;
        border-radius: 5px;
        background-color: #fff;
        margin-bottom: 10px;
        cursor: pointer;
        outline: 1px solid transparent;
        transition: all ease-in-out 200ms;
    }

    .house-btn:hover {
        outline: 1px solid #000;
    }

    .house-btn:last-child {
        margin-bottom: 0;
    }

    .house-btn-item {
        margin-right: 20px;
    }

    /* Floor Level */

    .floor-ctn {
        padding: 15px;
        border-radius: 5px;
        background: lightgray;
    }

    .floor-btn {
        display: flex;
        /* display: block; */
        padding: 15px;
        border-radius: 5px;
        background-color: #fff;
        margin-bottom: 10px;
        cursor: pointer;
        outline: 1px solid transparent;
        transition: all ease-in-out 200ms;
    }

    .floor-btn:hover {
        outline: 1px solid #000;
    }

    .floor-btn:last-child {
        margin-bottom: 0;
    }

    .floor-btn-item {
        margin-right: 20px;
    }
</style>
