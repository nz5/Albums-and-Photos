<template>
    <div class="homeCnt">
        <div class="columnCnt">
            <h2>List of Albums</h2>
            <div class="itemsCnt">
                <div v-for="(i, index) of Array(minNumOfAlbums)"
                     :key="`droppable-${index}`"
                     :id="`droppable-${index}`"
                     class="item albumCnt">Album - {{ index }}
                </div>
            </div>
        </div>
        <div class="columnCnt">
            <h2>List of Photos</h2>
            <div class="itemsCnt">
                <div v-for="(i, index) of Array(minNumOfPhotos)"
                     :key="`draggable-${index}`"
                     :id="`draggable-${index}`"
                     class="item photoCnt">Photo - {{ index }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Home",
        data() {
            return {
                minNumOfAlbums: 3,
                minNumOfPhotos: 5,
            };
        },
    }
</script>

<style scoped>
    .homeCnt {
        border: 2px solid red;
        padding: 50px 0;
        min-height: 100vh;
        display: flex;
        align-items: flex-start;
        justify-content: space-around;
    }

    .columnCnt {
        width: 40%;
        min-height: 300px;
        border: 1px solid white;
    }

    .itemsCnt {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        border: 1px solid blue;
    }

    .item {
        border-radius: 8px;
        width: 100px;
        height: 50px;
        margin-bottom: 10px;
        user-select: none;
    }

    .albumCnt {
        background-color: lightskyblue;
    }

    .photoCnt {
        background-color: palevioletred;
        cursor: move;
    }
</style>