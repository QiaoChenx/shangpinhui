<template>
    <div class="type-nav">
        <div class="container">
            <div @mouseleave="leaveCart">
                <h2 class="all" @mouseenter="handleShow">全部商品分类</h2>
                <transition name="ani">
                    <div class="sort" @click="handleSearch" v-show="show">
                        <div class="all-sort-list2">
                            <div
                                class="item bo"
                                v-for="(c1, index) of categoryList"
                                :key="c1.id"
                                :class="{ cur: currentIndex == index }"
                                v-show="index <= categoryList.length - 3"
                            >
                                <h3 @mousemove="changeIndex(index)">
                                    <a
                                        :data-categoryName="c1.categoryName"
                                        :data-category1Id="c1.categoryId"
                                        href="javascript:;"
                                        >{{ c1.categoryName }}</a
                                    >
                                </h3>
                                <div
                                    class="item-list clearfix"
                                    :style="{
                                        display:
                                            currentIndex == index
                                                ? 'block'
                                                : 'none',
                                    }"
                                >
                                    <div class="subitem">
                                        <dl
                                            class="fore"
                                            v-for="(
                                                c2, index
                                            ) of c1.categoryChild"
                                            :key="c2.categoryId"
                                        >
                                            <dt>
                                                <a
                                                    :data-categoryName="
                                                        c2.categoryName
                                                    "
                                                    :data-category2Id="
                                                        c2.categoryId
                                                    "
                                                    href="javascript:;"
                                                    >{{ c2.categoryName }}</a
                                                >
                                            </dt>
                                            <dd>
                                                <em
                                                    v-for="(
                                                        c3, index
                                                    ) of c2.categoryChild"
                                                    :key="c3.categoryId"
                                                >
                                                    <a
                                                        :data-categoryName="
                                                            c3.categoryName
                                                        "
                                                        :data-category3Id="
                                                            c3.categoryId
                                                        "
                                                        href="javascript:;"
                                                        >{{
                                                            c3.categoryName
                                                        }}</a
                                                    >
                                                </em>
                                            </dd>
                                        </dl>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </transition>
            </div>
            <nav class="nav">
                <a href="###">服装城</a>
                <a href="###">美妆馆</a>
                <a href="###">尚品汇超市</a>
                <a href="###">全球购</a>
                <a href="###">闪购</a>
                <a href="###">团购</a>
                <a href="###">有趣</a>
                <a href="###">秒杀</a>
            </nav>
        </div>
    </div>
</template>

<script>
import { mapState } from "vuex";
import throttle from "lodash/throttle";
export default {
    name: "NavType",
    data() {
        return {
            currentIndex: -1,
            show: true,
        };
    },
    mounted() {
        if (this.$route.path != "/home") {
            this.show = false;
        }
    },
    computed: {
        ...mapState({
            categoryList: (state) => state.home.categoryList,
        }),
    },
    methods: {
        changeIndex: throttle(function (index) {
            this.currentIndex = index;
        }, 50),
        leaveCart() {
            this.currentIndex = -1;

            if (this.$route.path != "/home") {
                this.show = false;
            }
        },
        handleSearch(e) {
            let { categoryname, category1id, category2id, category3id } =
                e.target.dataset;
            if (categoryname) {
                let location = {
                    name: "search",
                };
                let query = {
                    categoryName: categoryname,
                };
                if (category1id) {
                    query.category1Id = category1id;
                } else if (category2id) {
                    query.category2Id = category2id;
                } else {
                    query.category3Id = category3id;
                }
                location.query = query;
                if (this.$route.params) {
                    location.params = this.$route.params;
                }
                this.$router.push(location);
                this.show = false;
            }
        },
        handleShow() {
            if (this.$route.path != "/home") {
                this.show = true;
            }
        },
    },
};
</script>

<style lang="less" scoped>
.type-nav {
    border-bottom: 2px solid #e1251b;

    .container {
        width: 1200px;
        margin: 0 auto;
        display: flex;
        position: relative;

        .all {
            width: 210px;
            height: 45px;
            background-color: #e1251b;
            line-height: 45px;
            text-align: center;
            color: #fff;
            font-size: 14px;
            font-weight: bold;
        }

        .nav {
            a {
                height: 45px;
                margin: 0 22px;
                line-height: 45px;
                font-size: 16px;
                color: #333;
            }
        }

        .sort {
            position: absolute;
            left: 0;
            top: 45px;
            width: 210px;
            height: 461px;
            background: #fafafa;

            z-index: 999;

            .all-sort-list2 {
                .cur {
                    background: skyblue;
                }

                .item {
                    h3 {
                        overflow: hidden;
                        line-height: 31px;
                        font-size: 14px;
                        font-weight: 400;
                        padding: 0 20px;
                        margin: 0;

                        a {
                            color: #333;
                        }
                    }

                    .item-list {
                        display: none;
                        overflow: hidden;
                        position: absolute;
                        width: 734px;
                        min-height: 461px;
                        background: #f7f7f7;
                        left: 210px;
                        border: 1px solid #ddd;
                        top: 0;
                        z-index: 9999 !important;

                        .subitem {
                            float: left;
                            width: 650px;
                            padding: 0 4px 0 8px;

                            dl {
                                border-top: 1px solid #eee;
                                padding: 6px 0;
                                overflow: hidden;
                                zoom: 1;

                                &.fore {
                                    border-top: 0;
                                }

                                dt {
                                    float: left;
                                    width: 54px;
                                    line-height: 22px;
                                    text-align: right;
                                    padding: 3px 6px 0 0;
                                    font-weight: 700;
                                }

                                dd {
                                    float: left;
                                    width: 415px;
                                    padding: 3px 0 0;
                                    overflow: hidden;

                                    em {
                                        float: left;
                                        height: 14px;
                                        line-height: 14px;
                                        padding: 0 8px;
                                        margin-top: 5px;
                                        border-left: 1px solid #ccc;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

// .ani-enter-active {
//     animation: move 0.2s linear;
// }

// .ani-leave-active {
//     animation: move 0.2s reverse;
// }

// @keyframes move {
//     from {
//         opacity: 0;
//     }

//     to {
//         opacity: 1;
//     }
// }

.ani-enter {
    opacity: 0;
}

.ani-enter-to {
    opacity: 1;
}

.ani-enter-active {
    transition: all 0.2s linear;
}

.ani-leave-active {
    transition: all 0.2s reverse;
}
</style>
