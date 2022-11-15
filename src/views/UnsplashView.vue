<!-- @format -->

<template>
    <div>
        <HeaderCont />
        <TitleCont name1="Unsplash" name2="referce api" />
        <section class="unsplash__slider">
            <div class="container">
                <h2>Rander images</h2>
                <div class="unsplash__inner">
                    <Swiper
                        :slidesPerView="5"
                        :initialSlide="3"
                        :autoplay="{
                            delay: 2500,
                            disableOnInteraction: false,
                        }"
                        :loop="true"
                        :loopFillGroupWithBlank="true"
                        :effect="'cards'"
                        :grabCursor="true"
                        :modules="modules"
                        class="mySwiper"
                    >
                        <SwiperSlide v-for="slider in sliders" :key="slider.id">
                            <article class="card">
                                <a :href="slider.links.html" target="blank">
                                    <img :src="slider.urls.regular" :alt="slider.id" />
                                </a>
                            </article>
                        </SwiperSlide>
                    </Swiper>
                </div>
            </div>
        </section>
        <!-- unsplash__slider -->
        <section class="unsplash__search">
            <div class="container">
                <input type="search" id="search" placeholder="검색하세요" v-model="inputSearch" v-on:keyup.enter="inputTest()" />
                <button type="button" v-on:click="inputTest()">검색</button>
            </div>
        </section>
        <!-- unsplash__search -->
        <section class="unsplash__btns">
            <div class="container">
                <button type="button" class="btn" v-for="unsplashTag in unsplashTags" :key="unsplashTag" v-on:click="clickTest(unsplashTag)">
                    {{ unsplashTag }}
                </button>
            </div>
        </section>
        <!--  movie__btns-->

        <section class="cont__unsplash">
            <div class="container">
                <div class="unsplash__inner">
                    <div class="unsplash__search"></div>
                    <div class="unsplash__images">
                        <ul>
                            <li v-for="splash in splashes" :key="splash.id" :v-model="search">
                                <a :href="splash.links.html">
                                    <img :src="splash.urls.regular" :alt="splash.id" />
                                </a>
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

import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/effect-coverflow';
import 'swiper/css/pagination';
import { EffectCards, Pagination, Autoplay } from 'swiper';

export default {
    components: {
        Swiper,
        SwiperSlide,
        HeaderCont,
        FooterCont,
        TitleCont,
        ContactCont,
    },
    setup() {
        const splashes = ref([]);
        const sliders = ref([]);
        const search = ref(['landscape']);
        const inputSearch = ref([]);
        const unsplashTags = ['Sky', 'Sea', 'Moon', 'Star'];

        const clickTest = (value) => {
            search.value = value;
            inputSearch.value = '';
            SearchSplashes();
        };
        const inputTest = () => {
            search.value = inputSearch.value;
            SearchSplashes();
        };

        const SearchSplashes = () => {
            fetch(`https://api.unsplash.com/search/photos/?client_id=XNTqM2xvTnEFo9-LRypjJ0rmsasrQOcjWsqV6xl_UB4&query=${search.value}&per_page=30`)
                .then((response) => response.json())
                // .then((result) => console.log(result.results))
                .then((result) => {
                    splashes.value = result.results;
                    search.value = '';
                })
                .catch((error) => console.log('error', error));
        };

        const RandomSplashes = () => {
            fetch('https://api.unsplash.com/photos/random?client_id=XNTqM2xvTnEFo9-LRypjJ0rmsasrQOcjWsqV6xl_UB4&count=30')
                .then((response) => response.json())
                // .then((result) => console.log(result))
                .then((result) => (splashes.value = result))
                .catch((error) => console.log('error', error));
        };

        const SliderSplashes = () => {
            fetch('https://api.unsplash.com/photos/random?client_id=XNTqM2xvTnEFo9-LRypjJ0rmsasrQOcjWsqV6xl_UB4&count=5')
                .then((response) => response.json())
                // .then((result) => console.log(result))
                .then((result) => (sliders.value = result))
                .catch((error) => console.log('error', error));
        };

        RandomSplashes();
        SliderSplashes();
        return {
            modules: [EffectCards, Pagination, Autoplay],
            splashes,
            search,
            sliders,
            inputSearch,
            unsplashTags,
            SearchSplashes,
            RandomSplashes,
            SliderSplashes,
            clickTest,
            inputTest,
        };
    },
};
</script>

<style lang="scss">
.cont__unsplash {
    ul {
        column-count: 4;
        column-gap: 20px;
        width: 100%;
    }

    li {
        margin-bottom: 20px;

        img {
            border-radius: 10px;
        }
    }
}

.unsplash__slider {
    color: var(--black);
    h2 {
        font-size: 50px;
        margin-bottom: 20px;
    }
    .unsplash__inner {
        padding-bottom: 10px;
        margin-bottom: 50px;

        h2 {
            font-size: 30px;
        }
        a {
            color: var(--black);
        }
    }

    .swiper {
        width: 100%;
        padding-top: 50px;
        padding-bottom: 50px;
        margin-bottom: 10px;
        .swiper-slide {
            background-position: center;
            background-size: cover;
            width: 300px;
            height: 450px;
            border-radius: 10px;
            margin: 0 auto;

            .card {
                width: 300px;
                height: 450px;
                margin: 0 auto;
                img {
                    display: block;
                    border-radius: 10px;
                    width: 100%;
                    height: 100%;
                    object-fit: cover;
                }
            }
        }
    }
}

.unsplash__search {
    margin-bottom: 100px;
    .container {
        position: relative;
    }

    h2 {
        color: var(--black);
        font-size: 40px;
        text-indent: -9999px;
        height: 0;
    }
    input {
        bottom: 0;
        background: var(--bg-light);
        border: 1px solid var(--black);
        border-radius: 50px;
        color: var(--black);
        width: 100%;
        padding: 14px 30px;
        font-family: var(--font-kor2);
    }
    button {
        position: absolute;
        right: 6px;
        top: 6px;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        border: 0;
        font-family: var(--font-kor2);
        cursor: pointer;
        z-index: 100;
        background: var(--black);
        color: var(--white);
    }
}

.unsplash__btns {
    margin-bottom: 20px;
    .btn {
        background: var(--bg-dark);
        color: var(--white);
        border: 0;
        margin-right: 20px;
        font-family: var(--font-kor2);
        font-size: 16px;
        padding: 10px;
        border-radius: 10px;
        cursor: pointer;
    }
}
</style>
