<template>
    <div class="homeCnt">
        <div class="columnCnt">
            <h2>List of Albums</h2>
            <div class="itemsCnt">
                <div v-for="(i, index) of Array(minNumOfAlbums)"
                     :key="`droppable-${index}`"
                     :id="`droppable-${index}`"
                     class="item albumCnt droppable">Album - {{ index }}
                </div>
                <div class="item addItemButton" @click="minNumOfAlbums++">Add new</div>
            </div>
        </div>
        <div class="columnCnt">
            <h2>List of Photos</h2>
            <div class="itemsCnt">
                <div v-for="(i, index) of Array(minNumOfPhotos)"
                     :key="`draggable-${index}`"
                     :id="`draggable-${index}`"
                     draggable="true"
                     class="item photoCnt draggable">Photo - {{ index }}
                </div>
                <div class="item addItemButton" @click="minNumOfPhotos++">Add new</div>
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
        mounted() {
            this.draggableItems = document.querySelectorAll(".draggable");
            this.draggableItems.forEach((item) => {
                this.addDraggableEvents(item);
            });

            this.droppableItems = document.querySelectorAll(".droppable");
            this.droppableItems.forEach((item) => {
                this.addDroppableEvents(item);
            });
        },
        destroyed() {
            // TODO clean up event listeners
        },
        methods: {
            addDraggableEvents(item) {
                item.addEventListener("dragstart", this.handleDragStart, false);
                item.addEventListener("dragenter", this.handleDragEnter, false);
                item.addEventListener("dragleave", this.handleDragLeave, false);
                item.addEventListener("dragend", this.handleDragEnd, false);
            },
            addDroppableEvents(item) {
                item.addEventListener("dragover", this.handleDragOver, false);
                item.addEventListener("drop", this.handleDrop, false);
            },

            // ==================== draggable functions
            handleDragStart(e) {
                e.target.style.opacity = "0.4";
                e.dataTransfer.setData("text/plain", e.toElement.id);
            },
            handleDragEnter() {
                // console.log("handleDragEnter e: ", e);
            },
            handleDragLeave() {
                // console.log("handleDragLeave e: ", e);
            },
            handleDragEnd(e) {
                // console.log("handleDragEnd e: ", e);
                e.target.style.opacity = "1";
            },

            // ==================== droppable functions
            handleDragOver(e) {
                e.preventDefault();
                e.dataTransfer.effectAllowed = "all";
                e.dataTransfer.dropEffect = "copy";
            },
            handleDrop(e) {
                e.preventDefault();
                const dragElemId = e.dataTransfer.getData("text/plain");
                const dropElemId = e.toElement.id;
                const dragElemLocation = document.getElementById(dragElemId).getBoundingClientRect();
                const dropElemLocation = document.getElementById(dropElemId).getBoundingClientRect();
                console.log("dragElemLocation ", dragElemLocation);
                console.log("dropElemLocation ", dropElemLocation);
            },
        }
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

    .addItemButton {
        border: 2px dotted black;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
    }
</style>