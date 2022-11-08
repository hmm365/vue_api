<template>
    <section class="cont__title">
        <div class="container">
            <h1>
                <strong>{{ title }}</strong>
                <em>{{ desc }}</em>
            </h1>
        </div>
    </section>
</template>
<style lang="scss"></style>
<script>
export default {
    props: {
        title: String,
        desc: String,
    },
};
</script>
