<template>
    <div id="indexHeader">
        <header>
            <div class="top_bar">
                <div class="abs_l"></div>
                <div class="abs_m"><a class='title' @click.stop="goTop"></a></div>
                <div class="abs_r"><a class='search_btn' slot='right' @click.stop="$router.push('/search')"></a></div>
            </div>
        </header>
        <nav>
            <div class="nav_ul">
                <a href='javascript:;' v-for="(item,index) in column" :class='{active: indexActive == item.classpath}' @click="navClick(item.classpath)" :key="index">{{item.classname}}</a>
            </div>
            <!-- <div class="nav_menu">
                <div class="shadow"></div>
                <a href='javascript:;' class="more_btn" @click="$router.push('/index/channel')"></a>
            </div> -->
        </nav>
    </div>
</template>
<script>
import { mapGetters , mapMutations } from 'vuex'
export default {
    props: ['column'],
    computed: {
        ...mapGetters('index', [
            'indexActive',
            'activeIndex',
        ]),
    },
    watch: {
        indexActive() {
            this.slideTo(this.activeIndex);
        },
    },
    methods: {
        ...mapMutations('index', [
            'set_indexActive',
        ]),
        goTop() {
            $(`.container.${this.indexActive}`).animate({scrollTop: 0});
        },
        slideTo(index) {
            this.$nextTick(() => {
                let $ul = $(".nav_ul");
                let $activeEle = $('.nav_ul a').eq(index);
                if ($activeEle.length == 0) {
                    return
                } else {
                    let move;
                    let ulWitch = $ul.width();
                    let aWidth = $activeEle.width();
                    let midWidth = (ulWitch - aWidth) / 2; // 屏幕中心线的宽度
                    let ullLeft = $ul.scrollLeft(); // ul 滚动条的值
                    let aLeft = $activeEle.position().left; // $activeEle 距离屏幕左边的距离
                    if (ullLeft === 0 && aLeft <= midWidth) {
                        move = 0;
                    } else {
                        move = ullLeft + (aLeft - midWidth);
                    }
                    sessionStorage.setItem('navScrollLeft', move);
                    $ul.animate({
                        'scrollLeft': move
                    }, 300);
                }
            })
        },
        recover() {
            let move = sessionStorage.getItem('navScrollLeft');
            if (move) {
                this.$nextTick(function() {
                    $('.nav_ul').scrollLeft(move);
                })
            }
        },
        navClick(type) {
            this.set_indexActive(type);
        }
    },
    activated() {
        this.recover();
    },
}
</script>
<style scoped lang='stylus'>
#indexHeader {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 999;
    overflow: hidden;
    header {
        display: block;
        position: relative;
        overflow: hidden;
        background-color: #00939c;
        color: #fff;
        &.fixed{
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 1000;
        }
        .title {
            background-size: 145px;
        }
        .search_btn {
            background-size: 24px;
        }
        .top_bar {
            position: relative;
            height: 44px;
            line-height: 44px;
            user-select:none;
            a{
                display: block;
                width: 100%;
                height: 100%;
                color: inherit;
                font-size: inherit;
                font-weight: inherit;
            }
            .abs_l,.abs_m,.abs_r {
                position: absolute;
                top: 0;
                width: 44px;
                height: 100%;
                font-size: inherit;
                color: inherit;
                text-align: center
            }
            .abs_l {
                left: 0;
                z-index: 1000;
            }
            .abs_m {
                width: 100%;
                font-weight: 700;
                z-index: 999;
            }
            .abs_r {
                right: 0;
                z-index: 1000;
            }
        }
    }
    nav {
        position: relative;
        height: 36px;
        line-height: 36px;
        background-color: #f4f5f6;
        border-bottom: 1px solid #ddd;
        /*padding-right: 40px;*/
        .nav_ul {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            white-space: nowrap;
            &::-webkit-scrollbar {
                width: 0px;
                height: 0px;
            }
            a {
                display: inline-block;
                padding-left: 10px;
                padding-right: 10px;
                margin-left: 5px;
                height: 36px;
                line-height: 36px;
                font-size: 17px;
                color: #505050;
                white-space: nowrap;
                -webkit-tap-highlight-color: rgba(0, 0, 0, .3);
                text-decoration: none;
                &:last-child {
                    margin-right: 5px;
                }
                &.active {
                    color: #00939c;
                    border-bottom: 2px solid #00939c;
                }
            }
        }
        .nav_menu {
            position: absolute;
            top: 0;
            right: 0;
            height: 100%;
            .shadow {
                position: absolute;
                width: 10px;
                height: 100%;
                left: -10px;
                background-size: contain;
                background-color: rgba(244, 245, 246, .3);
            }
            .more_btn {
                display: block;
                width: 40px;
                height: 100%;
                background-size: 20px;
                background-color: #f4f5f6;
            }
        }
    }
}
</style>
<style scoped>
.title {
    background: url(~@/assets/img/news_logo.png)no-repeat center center;
    background-size: contain;
}

.search_btn {
    background: url(~@/assets/img/search.png)no-repeat center center;
}

.shadow {
    background: url(~@/assets/img/shadow.png) no-repeat 100%;
}

.more_btn {
    background: url(~@/assets/img/menu_more.png) no-repeat 50%;
}
</style>
