<template>
    <div class="homeCnt">
        <canvas :id="canvasId" class="canvas"></canvas>
        <div class="mainCnt">
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
    </div>
</template>

<script>
    export default {
        name: "Home",
        data() {
            return {
                canvasId: "my-canvas-id",
                minNumOfAlbums: 3,
                minNumOfPhotos: 5,
                albums: []
            };
        },
        mounted() {
            this.resizeCanvasToDisplaySize(document.getElementById(this.canvasId));

            this.draggableItems = document.querySelectorAll(".draggable");
            this.draggableItems.forEach((item) => {
                this.addDraggableEvents(item);
            });

            this.droppableItems = document.querySelectorAll(".droppable");
            this.droppableItems.forEach((item) => {
                this.addAlbumToTheList(item.id);
                this.addDroppableEvents(item);
            });
        },
        destroyed() {
            // TODO clean up event listeners
        },
        methods: {
            // ==================== canvas functions
            handleWindowResize() {
                const canvas = document.getElementById(this.canvasId);
                const ctx = canvas.getContext("2d");
                ctx.canvas.width = window.innerWidth;
                ctx.canvas.height = window.innerHeight;
            },
            drawOnCanvas(fromX, fromY, toX, toY) {
                const canvas = document.getElementById(this.canvasId);
                const ctx = canvas.getContext("2d");
                ctx.beginPath();
                ctx.lineWidth = "1";
                ctx.moveTo(fromX, fromY);
                ctx.lineTo(toX, toY);
                ctx.stroke();
            },
            resizeCanvasToDisplaySize(canvas) {
                // Lookup the size the browser is displaying the canvas in CSS pixels.
                const displayWidth = canvas.clientWidth;
                const displayHeight = canvas.clientHeight;

                const needResize = canvas.width !== displayWidth ||
                    canvas.height !== displayHeight;

                if (needResize) {
                    canvas.width = displayWidth;
                    canvas.height = displayHeight;
                }
            },

            // ==================== event functions
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
            handleDragEnd(e) {
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
                this.drawOnCanvas(dropElemLocation.right,
                    dropElemLocation.top + dropElemLocation.height / 2,
                    dragElemLocation.left,
                    dragElemLocation.top + dragElemLocation.height / 2);
                this.addPhotoToAlbum(dragElemId, dropElemId);
            },
            addAlbumToTheList(albumId) {
                this.albums.push(
                    {
                        albumId: albumId,
                        photoIds: []
                    }
                );
            },
            addPhotoToAlbum(photoId, albumId) {
                let album = this.albums.find(item => {
                    return item.albumId === albumId;
                })
                album.photoIds.push(photoId);
                const photoElem = document.getElementById(photoId);
                photoElem.draggable = false;
            }
        },
        watch: {
            minNumOfAlbums: {
                handler(newValue) {
                    // hacky way of waiting until new component added to the DOM. #time-pressure
                    setTimeout(() => {
                        const lastIndex = newValue - 1;
                        this.droppableItems = document.querySelectorAll(".droppable");
                        this.droppableItems.forEach((item, index) => {
                            if (index === lastIndex) {
                                this.addAlbumToTheList(item.id);
                                this.addDroppableEvents(item);
                            }
                        });
                    }, 100)
                },
            },
            minNumOfPhotos: {
                handler(newValue) {
                    // hacky way of waiting until new component added to the DOM. #time-pressure
                    setTimeout(() => {
                        const lastIndex = newValue - 1;
                        this.draggableItems = document.querySelectorAll(".draggable");
                        this.draggableItems.forEach((item, index) => {
                            if (index === lastIndex) {
                                this.addDraggableEvents(item);
                            }
                        });
                    }, 100);
                },
            },
        }
    }
</script>

<style scoped>
    .homeCnt {
        border: 2px solid red;
        padding: 50px 0;
        min-height: 100vh;
    }

    .canvas {
        position: absolute;
        z-index: 1;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        border: 2px solid green;
    }

    .mainCnt {
        position: relative;
        z-index: 1;
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