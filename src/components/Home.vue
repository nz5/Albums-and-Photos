<template>
    <div class="homeCnt">
        <div class="columnCnt">
            <h2>List of Albums</h2>
            <div class="itemsCnt">
                <div class="item albumCnt">Album 1</div>
                <div class="item albumCnt">Album 2</div>
                <div class="item albumCnt">Album 3</div>
            </div>
        </div>
        <div class="columnCnt">
            <h2>List of Photos</h2>
            <div class="itemsCnt">
                <div class="item photoCnt">Photo 1</div>
                <div class="item photoCnt">Photo 2</div>
                <div class="item photoCnt">Photo 3</div>
                <div class="item photoCnt">Photo 4</div>
                <div class="item photoCnt">Photo 5</div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Home"
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