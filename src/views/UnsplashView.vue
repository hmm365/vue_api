<!-- @format -->

<template>
    <div>
        <HeaderCont />
        <TitleCont name1="Unsplash" name2="referce api" />
        <section class="cont__unsplash">
            <div class="container">
                <div class="unsplash__inner">
                    <div class="unsplash__slider"></div>
                    <div class="unsplash__search"></div>
                    <div class="unsplash__images">
                        <ul>
                            <li v-for="splash in splashes" :key="splash.id">
                                <img :src="splash.urls.regular" :alt="splash.id" />
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        <FooterCont />
        <ContactCont />
    </div>
</template>

<script>
import HeaderCont from '@/components/HeaderCont.vue';
import FooterCont from '@/components/FooterCont.vue';
import TitleCont from '@/components/TitleCont.vue';
import ContactCont from '@/components/ContactCont.vue';
import { ref } from 'vue';
export default {
    components: {
        HeaderCont,
        FooterCont,
        TitleCont,
        ContactCont,
    },
    setup() {
        const splashes = ref([]);
        const search = ref(['landscape']);

        const SearchSplashes = () => {
            fetch(`https://api.unsplash.com/search/photos?client_id=XNTqM2xvTnEFo9-LRypjJ0rmsasrQOcjWsqV6xl_UB4&query=${search.value}`)
                .then((response) => response.json())
                // .then((result) => console.log(result.results))
                .then((result) => (search.value = result.results))
                .catch((error) => console.log('error', error));
        };

        const RandomSplashes = () => {
            fetch('https://api.unsplash.com/photos/random?client_id=XNTqM2xvTnEFo9-LRypjJ0rmsasrQOcjWsqV6xl_UB4&count=20')
                .then((response) => response.json())
                .then((result) => console.log(result))
                .then((result) => (splashes.value = result))
                .catch((error) => console.log('error', error));
        };
        SearchSplashes();
        RandomSplashes();
        return {
            splashes,
            search,
            SearchSplashes,
            RandomSplashes,
        };
    },
};
</script>
