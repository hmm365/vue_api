<template>
    <section class="cont__title">
        <div class="container">
            <h1>
                <strong>{{ name1 }}</strong>
                <em>{{ name2 }}</em>
            </h1>
        </div>
    </section>
</template>
<style lang="scss"></style>
<script>
export default {
    props: {
        name1: String,
        name2: String,
    },
};
</script>
