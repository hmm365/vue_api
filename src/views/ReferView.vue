<!-- @format -->

<template>
    <div>
        <HeaderCont />
        <TitleCont name1="CSS" name2="referce api" />
        <section class="cont__refer">
            <div class="container">
                <div class="refer__inner">
                    <h2>CSS</h2>
                    <div class="refer__list">
                        <li v-for="refer in refers" :key="refer.num">
                            <a href="/">
                                <span className="num">{{ refer.num }}</span>
                                <span className="name">{{ refer.title }}</span>
                                <span className="desc">{{ refer.desc }}</span>
                                <span className="star">{{ refer.descStar }}</span>
                            </a>
                        </li>
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
        const refers = ref([]);

        const referces = () => {
            fetch('https://hmm365.github.io/react_api/src/utils/reference.json')
                .then((response) => response.json())
                // .then((result) => console.log(result.cssRefer))
                .then((result) => (refers.value = result.cssRefer))
                .catch((error) => console.log('error', error));
        };
        referces();

        return {
            refers,
            referces,
        };
    },
};
</script>

<style lang="scss">
.refer__inner {
    padding-bottom: 200px;
    h2 {
        font-size: 30px;
        color: var(--black);
    }
}

.refer__list {
    border: 1px solid var(--bg-light-border);
    li {
        list-style: none;
        border-bottom: 1px solid var(--bg-light-border);
        a {
            display: flex;
            align-items: center;
            width: 100%;
            color: var(--black);
            span {
                font-family: var(--font-kor2);
                padding: 15px 20px;
                display: inline-block;
                font-weight: 400;
            }

            .num {
                flex: 1 1 10%;
                border-right: 1px solid var(--bg-light-border);
            }
            .name {
                flex: 1 1 25%;
                border-right: 1px solid var(--bg-light-border);

                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
            }
            .desc {
                flex: 1 1 55%;
                border-right: 1px solid var(--bg-light-border);

                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
            }
            .star {
                flex: 1 1 10%;
            }
        }
    }
}
</style>
