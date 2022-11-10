<!-- @format -->

<template>
    <div>
        <HeaderCont />
        <TitleCont name1="movie" name2="referce api" />

        <div class="popular__movie">
            <div class="container">
                <h2>Best Movie</h2>
                <div class="movie__inner">
                    <Swiper
                        :initialSlide="5"
                        :autoplay="{
                            delay: 2500,
                            disableOnInteraction: false,
                        }"
                        :effect="'coverflow'"
                        :grabCursor="true"
                        :centeredSlides="true"
                        :slidesPerView="'auto'"
                        :coverflowEffect="{
                            rotate: 50,
                            stretch: 0,
                            depth: 100,
                            modifier: 1,
                            slideShadows: false,
                        }"
                        :pagination="true"
                        :modules="modules"
                        class="mySwiper"
                    >
                        <Swiper-slide v-for="slider in sliders" :key="slider.id">
                            <figure className="card">
                                <a :href="`https://www.themoviedb.org/movie/${slider.id}`">
                                    <img :src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`" :alt="`${slider.original_title} poster`" />
                                </a>
                            </figure>
                        </Swiper-slide>
                    </Swiper>
                </div>
            </div>
        </div>

        <div class="movie__search">
            <div class="container">
                <form @submit.prevent="SearchMovies()">
                    <input type="search" id="search" placeholder="검색하세요" v-model="search" />
                    <button type="submit">검색</button>
                </form>
            </div>
        </div>
        <!-- movie__search -->

        <section class="cont__movie">
            <div class="container">
                <div class="movie__inner">
                    <article class="card" v-for="movie in movies" :key="movie.id" :v-model="search">
                        <figure class="card__top">
                            <a :href="`https://www.themoviedb.org/movie/${movie.id}`">
                                <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" :alt="`${movie.original_title} poster`" />
                            </a>
                        </figure>
                        <div class="card__bottom">
                            <a :href="`https://www.themoviedb.org/movie/${movie.id}`">{{ movie.title }}</a>
                            <p class="vote_average">평점 : {{ movie.vote_average }}</p>
                            <p class="releasedate">{{ movie.release_date }}</p>
                        </div>
                    </article>
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
import { EffectCoverflow, Pagination, Autoplay } from 'swiper';

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
        const movies = ref([]);
        const sliders = ref([]);
        const search = ref(['마블']);

        const SearchMovies = async () => {
            await fetch(
                `https://api.themoviedb.org/3/search/movie?api_key=891244fed71f3e78a463eb2a1801b383&language=ko-kr&page=1&include_adult=false&query=${search.value}`,
            )
                .then((response) => response.json())
                // .then((result) => console.log(result.results))
                .then((result) => {
                    movies.value = result.results;
                    search.value = '';
                })
                .catch((error) => console.log('error', error));
        };

        const TopMovies = () => {
            fetch(`https://api.themoviedb.org/3/movie/popular?api_key=891244fed71f3e78a463eb2a1801b383&language=ko-kr&page=1`)
                .then((response) => response.json())
                // .then((result) => console.log(result.results))
                .then((result) => (sliders.value = result.results))
                .catch((error) => console.log('error', error));
        };
        SearchMovies();
        TopMovies();
        return {
            modules: [EffectCoverflow, Pagination, Autoplay],
            movies,
            sliders,
            search,
            SearchMovies,
            TopMovies,
        };
    },
};
</script>

<style lang="scss">
.cont__movie {
    .movie__inner {
        padding-bottom: 200px;
        display: flex;
        flex-wrap: wrap;
        align-content: center;
        justify-content: space-around;
        h2 {
            font-size: 30px;
            color: var(--black);
        }
        a {
            color: var(--black);
        }
        .card {
            width: 18%;
            margin-bottom: 40px;
            color: var(--black);
            font-family: var(--font-kor1);

            .card__top {
                margin-bottom: 10px;
                transition: 0.6s ease;
                border-radius: 10px;
                img {
                    border-radius: 10px;
                    width: 100%;
                    object-fit: cover;
                }
            }
            .card__bottom {
                transition: 0.6s ease;
                .title {
                    display: block;
                    color: var(--black);
                    margin-bottom: 5px;
                    font-size: 1.3vw;
                    opacity: 0.8;

                    width: 100%;
                    overflow: hidden;
                    black-space: nowrap;
                    text-overflow: ellipsis;
                }
                .vote_average {
                    font-size: 1.1vw;
                    opacity: 0.6;
                }
                .releasedate {
                    font-size: 1vw;
                    opacity: 0.6;
                }
            }

            &:hover .card__top,
            &:hover .card__bottom {
                transform: translateY(-5px);
            }
        }
    }
}

.movie__search {
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
    }
}

.popular__movie {
    color: var(--black);
    h2 {
        font-size: 50px;
        margin-bottom: 20px;
    }
    .movie__inner {
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
            // background-position: center;
            // background-size: cover;
            width: 300px;
            height: 450px;
            border-radius: 10px;

            .card {
                img {
                    display: block;
                    border-radius: 10px;
                    width: 300px;
                    height: 450px;

                    object-fit: cover;
                }
            }
        }
    }
}
</style>
