<script>
    import Bed from "../lib/Bed.svelte";
    import Dressing from "../lib/Dressing.svelte";
    import Tv from "../lib/Tv.svelte";

    let floorIndex = null;
    let roomIndex = null;
    let furnitureIndex = null;
    let addBtnRoomIndex = null;

    let data = [
        {
            name: "Maison 1",
            floors: [
                {
                    name: "Etage n°1",
                    rooms: [
                        {
                            name: "Chambre n°1",
                            surface: 10,
                            furnitures: [
                                {
                                    bois: "Chêne",
                                    name: "Lit n°1",
                                    type: "bed",
                                },
                                {
                                    name: "Dressing n°1",
                                    type: "dressing",
                                    surface: 20,
                                },
                                {
                                    name: "Dressing n°2",
                                    type: "dressing",
                                    surface: 10,
                                },
                                {
                                    name: "TV n°1",
                                    type: "tv",
                                    definition: "4K",
                                    dimensions: [20, 30],
                                },
                            ],
                        },
                        {
                            name: "Chambre n°2",
                            surface: 20,
                            furnitures: [
                                {
                                    bois: "Chêne",
                                    name: "Lit n°1",
                                    type: "bed",
                                },
                                {
                                    name: "Dressing n°1",
                                    type: "dressing",
                                    surface: 30,
                                },
                                {
                                    name: "Dressing n°2",
                                    type: "dressing",
                                    surface: 40,
                                },
                                {
                                    name: "TV n°1",
                                    type: "tv",
                                    definition: "4K",
                                    dimensions: [30, 40],
                                },
                            ],
                        },
                    ],
                    surface: 100,
                },
                {
                    name: "Etage n°2",
                    rooms: [
                        {
                            name: "Chambre n°1",
                            surface: 10,
                            furnitures: [
                                {
                                    bois: "Chêne",
                                    name: "Lit n°1",
                                    type: "bed",
                                },
                                {
                                    name: "Dressing n°1",
                                    type: "dressing",
                                    surface: 20,
                                },
                                {
                                    name: "Dressing n°2",
                                    type: "dressing",
                                    surface: 10,
                                },
                                {
                                    name: "TV n°1",
                                    type: "tv",
                                    definition: "4K",
                                    dimensions: [20, 30],
                                },
                            ],
                        },
                    ],
                    surface: 200,
                },
            ],
        },
    ];

    function setFloorIndex(index) {
        floorIndex = index;
        console.log(floorIndex);
    }

    function setRoomIndex(index) {
        roomIndex = index;
        console.log(roomIndex);
    }

    function setFurnitureIndex(index) {
        furnitureIndex = index;
        console.log(furnitureIndex);
    }

    function setAddBtnRoomIndex(index) {
        furnitureIndex = index;
        console.log(addBtnRoomIndex);
    }

    function addItem(item, b, c) {
        if (item === "floor") {
            const newItemIndex = data[0].floors.length + 1;
            data[0].floors = [
                ...data[0].floors,
                { name: "Etage n°" + newItemIndex, rooms: [] },
            ];
        } else if (item === "room") {
            if (floorIndex !== b) {
                return;
            }
            const newItemIndex = data[0].floors[floorIndex].rooms.length + 1;
            data[0].floors[floorIndex].rooms = [
                ...data[0].floors[floorIndex].rooms,
                { name: "Pièce n°" + newItemIndex, furnitures: [] },
            ];
        } else if (item === "furniture") {
            if (floorIndex !== b || roomIndex !== c) {
                return;
            }
            const newItemIndex =
                data[0].floors[floorIndex].rooms[roomIndex].furnitures.length +
                1;
            data[0].floors[floorIndex].rooms[roomIndex].furnitures = [
                ...data[0].floors[floorIndex].rooms[roomIndex].furnitures,
                { name: "Meuble n°" + newItemIndex },
            ];
        }
    }
</script>

{#each data as property, a}
    <div class="ctn">
        {#each property.floors as floor, b}
            <button
                class="btn"
                on:click|self={() => setFloorIndex(b)}
                on:click|self={() => setRoomIndex(null)}
                on:click|self={() => setFurnitureIndex(null)}
            >
                <div>{floor.name}</div>
                <div
                    class="ctn"
                    class:selected={floorIndex === b && roomIndex === null}
                >
                    {#each floor.rooms as room, c}
                        <button
                            class="btn"
                            on:click|self={() => setFloorIndex(b)}
                            on:click|self={() => setRoomIndex(c)}
                            on:click|self={() => setFurnitureIndex(null)}
                        >
                            <div>{room.name}</div>
                            <div
                                class="ctn"
                                class:selected={floorIndex === b &&
                                    roomIndex === c &&
                                    furnitureIndex === null}
                            >
                                {#each room.furnitures as furniture, d}
                                    <button
                                        class="btn"
                                        on:click|self={() => setFloorIndex(b)}
                                        on:click|self={() => setRoomIndex(c)}
                                        on:click|self={() =>
                                            setFurnitureIndex(d)}
                                    >
                                        <div>{furniture.name}</div>
                                        <div
                                            class="ctn"
                                            class:selected={floorIndex === b &&
                                                roomIndex === c &&
                                                furnitureIndex === d}
                                        >
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
                                    </button>
                                {/each}
                                <button
                                    class="add-btn"
                                    on:click={() => addItem("furniture", b, c)}
                                    >Ajouter un meuble</button
                                >
                            </div>
                        </button>
                    {/each}
                    <button class="add-btn" on:click={() => addItem("room", b)}
                        >Ajouter une pièce</button
                    >
                </div>
            </button>
        {/each}
        <button class="add-btn" on:click={() => addItem("floor")}
            >Ajouter un étage</button
        >
    </div>
{/each}

<div class="ctn room-list-ctn">
    <ul>
        <li><button>Chambre</button></li>
        <li><button>Cuisine</button></li>
        <li><button>Salle de bain</button></li>
    </ul>
</div>

<style>
    .selected {
        background-color: #ff3e00 !important;
    }

    .visible {
        visibility: visible;
    }
    .hidden {
        visibility: hidden;
    }

    .ctn {
        padding: 15px;
        border-radius: 5px;
        background: lightgray;
        width: 90%;
    }

    .room-list-ctn {
        position: fixed;
        right: 0;
        top: 0;
        width: 100px;
        border: 1px solid #000;
        border-radius: 0px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .add-btn {
        display: block;
        background-color: #0079f2;
        color: #fff;
        border-radius: 10px;
        padding: 12px 12px;
        cursor: pointer;
    }
    .add-btn:active {
        background-color: #0066cc;
    }

    .btn {
        display: flex;
        width: 100%;
        border: none;
        padding: 20px;
        margin-bottom: 10px;
        border-radius: 10px;
        cursor: pointer;
        background-color: #fff;
    }

    .btn:hover {
        outline: 1px solid #000;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }
</style>
