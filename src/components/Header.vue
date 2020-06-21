<template>
    <div class="header">
        {{title}}
    </div>
</template>
<script>
export default {
    name: 'Header',
    props: {
        title: String,
    }
}
</script>
<style lang="scss" scoped>
    .header {
        width: 100vw;
        height: 22px;
        // position: fixed;
        // top: 0;
        background: linear-gradient(45deg, #92fe9d, #00c9ff);
        display: flex;
        color: #fff;
        font-weight: bold;
        justify-content: center;
        align-items: center;  
        padding-top: 10px;
        padding-bottom: 10px; 
    }
</style>