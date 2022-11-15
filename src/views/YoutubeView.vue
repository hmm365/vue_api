<!-- @format -->
<template>
    <div>
        <HeaderCont />
        <TitleCont name1="Youtube" name2="referce api" />

        <section class="youtube__slider">
            <div class="container">
                <h2>Rander images</h2>
                <div class="youtube__inner">
                    <Swiper
                        :slidesPerView="1"
                        :spaceBetween="30"
                        :loop="true"
                        :pagination="{
                            clickable: true,
                        }"
                        :navigation="true"
                        :modules="modules"
                        class="mySwiper"
                    >
                        <SwiperSlide v-for="slider in sliders" :key="slider.id.videoId">
                            <article class="card">
                                <a :href="`https://www.youtube.com/watch?v=${slider.id.videoId}`" target="blank">
                                    <img :src="slider.snippet.thumbnails.high.url" :alt="slider.snippet.description" />
                                </a>
                            </article>
                        </SwiperSlide>
                    </Swiper>
                </div>
            </div>
        </section>
        <!-- youtube__slider -->

        <section class="youtube__search">
            <div class="container">
                <input type="search" id="search" placeholder="검색하세요" v-model="inputSearch" v-on:keyup.enter="inputTest()" />
                <button type="button" v-on:click="inputTest()">검색</button>
            </div>
        </section>
        <!-- youtube__search -->

        <section class="youtube__btns">
            <div class="container">
                <button type="button" class="btn" v-for="youtubeTag in youtubeTags" :key="youtubeTag" v-on:click="clickTest(youtubeTag)">
                    {{ youtubeTag }}
                </button>
            </div>
        </section>
        <!--  movie__btns-->

        <section class="cont__youtube">
            <div class="container">
                <div class="youtube__inner">
                    <div class="youtube__search"></div>
                    <div class="youtube__images">
                        <ul>
                            <li v-for="youtube in youtubes" :key="youtube.id.videoId" :v-model="search">
                                <a :href="`https://www.youtube.com/watch?v=${youtube.id.videoId}`" target="blank">
                                    <img :src="youtube.snippet.thumbnails.high.url" :alt="youtube.snippet.description" />
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
import { Pagination, Navigation } from 'swiper';
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
        const youtubes = ref([]);
        const sliders = ref([]);
        const search = ref(['소각소각']);
        const inputSearch = ref([]);
        const youtubeTags = ['이바다', '소각소각', '윤하', '리엑트'];

        const clickTest = (value) => {
            search.value = value;
            inputSearch.value = '';
            SearchYoutube();
        };
        const inputTest = () => {
            search.value = inputSearch.value;
            SearchYoutube();
        };

        const SearchYoutube = () => {
            fetch(
                `https://www.googleapis.com/youtube/v3/search?key=AIzaSyCehmcOOsAQuFQZYZta2uvoJARWdyVGVyQ&q=${search.value}&part=snippet&maxResults=30&type=video`,
            )
                .then((response) => response.json())
                // .then((result) => console.log(result.items))
                .then((result) => {
                    youtubes.value = result.items;
                    search.value = '';
                })
                .catch((error) => console.log('error', error));
        };

        const SliderYoutube = () => {
            fetch('https://www.googleapis.com/youtube/v3/search?key=AIzaSyCehmcOOsAQuFQZYZta2uvoJARWdyVGVyQ&q=이바다&part=snippet&maxResults=10&type=video')
                .then((response) => response.json())
                // .then((result) => console.log(result.items))
                .then((result) => (sliders.value = result.items))
                .catch((error) => console.log('error', error));
        };

        SearchYoutube();
        SliderYoutube();
        return {
            modules: [Pagination, Navigation],
            youtubes,
            search,
            sliders,
            inputSearch,
            youtubeTags,
            SearchYoutube,
            SliderYoutube,
            clickTest,
            inputTest,
        };
    },
};
</script>

<style lang="scss">
.cont__youtube {
    ul {
        column-count: 4;
        column-gap: 20px;
        width: 100%;
    }

    li {
        margin-bottom: 20px;

        img {
            width: 100%;
            height: 100%;
            border-radius: 10px;
        }
    }
}

.youtube__slider {
    color: var(--black);
    h2 {
        font-size: 50px;
        margin-bottom: 20px;
    }
    .youtube__inner {
        padding-bottom: 10px;
        margin-bottom: 50px;

        h2 {
            font-size: 30px;
        }
        a {
            color: var(--white);
        }
    }

    .swiper {
        width: 60%;
        padding-top: 50px;
        padding-bottom: 50px;
        margin-bottom: 10px;
        .swiper-slide {
            // background-position: center;
            // background-size: cover;
            border-radius: 10px;

            .card {
                img {
                    display: block;
                    border-radius: 10px;
                    object-fit: cover;
                }
            }
        }
    }
}

.youtube__search {
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

.youtube__btns {
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
