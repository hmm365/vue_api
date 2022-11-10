<!-- @format -->
<template>
    <div>
        <HeaderCont />
        <TitleCont name1="Youtube" name2="referce api" />
        <FooterCont />
        <ContactCont />
    </div>
</template>

<script>
import HeaderCont from '@/components/HeaderCont.vue';
import FooterCont from '@/components/FooterCont.vue';
import TitleCont from '@/components/TitleCont.vue';
import ContactCont from '@/components/ContactCont.vue';
export default {
    components: {
        HeaderCont,
        FooterCont,
        TitleCont,
        ContactCont,
    },
};
</script>
