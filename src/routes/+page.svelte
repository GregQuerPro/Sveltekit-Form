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
        <div class="house-ctn show">
            {#each property.floors as floor, a}
                <div
                    class="house-btn"
                    data-floor={a}
                    on:keypress={handleFloorCtnClick}
                >
                    <div class="house-btn-item">{floor.name}</div>
                    <div class="floor-ctn hide" data-floor={a}>
                        {#each floor.rooms as room}
                            <div class="floor-btn">
                                <div class="floor-item">{room.name}</div>
                                <div class="room-ctn hide" data-floor={a}>
                                    {#each room.furnitures as furniture}
                                        <div class="room-btn">
                                            <div class="room-item">
                                                <div>{furniture.name}</div>
                                                {#if furniture.type === "bed"}
                                                    <Bed bed={furniture} />
                                                {:else if furniture.type === "dressing"}
                                                    <Dressing
                                                        dressing={furniture}
                                                    />
                                                {:else if furniture.type === "tv"}
                                                    <Tv tv={furniture} />
                                                {/if}
                                            </div>
                                        </div>
                                    {/each}
                                </div>
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
    .show {
        position: fixed;
        top: 100px;
        right: 50%;
        width: 90%;
        max-width: 900px;
        transform: translateX(50%);
    }

    .hide {
        position: fixed;
        left: -1000px;
    }

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

    /* Room Level */

    .room-ctn {
        padding: 15px;
        border-radius: 5px;
        background: lightgray;
    }

    .room-btn {
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

    .room-btn:hover {
        outline: 1px solid #000;
    }

    .room-btn:last-child {
        margin-bottom: 0;
    }

    .room-btn-item {
        margin-right: 20px;
    }
</style>
