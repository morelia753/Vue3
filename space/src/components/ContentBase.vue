<template>
    <div class = "home">
        <div class = "container">
            <div class="card">
                <div class="card-body">
                    <!-- 子类 -->
                    <slot></slot>
                </div>
            </div>
        </div>
    </div>
</template>
    
<script>
export default {
    name: "ContentBase",
}
</script>

<style scoped>
.container {
    margin-top: 20px;
}
</style>