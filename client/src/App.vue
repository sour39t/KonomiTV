<template>
    <v-app id="app">
        <transition>
            <router-view/>
        </transition>
    </v-app>
</template>
<style lang="scss">

// 全体のスタイル
html {
    overflow-y: auto !important;
    touch-action: manipulation;
}

// アプリケーションのルート
body .v-application {
    min-height: 100vh;
    min-height: 100dvh;
    font-family: 'Open Sans', 'YakuHanJPs', 'Hiragino Sans', 'Noto Sans JP', sans-serif;
    font-weight: 500;
    overflow-x: clip;  // clip なら position: sticky; が効く
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;

    .v-application--wrap {
        min-height: 100% !important;
    }
}

// ヘッダー以外のメインコンテンツのルート
body main {
    display: flex;
    width: 100%;
    min-height: 100%;
}
body header + main {
    // ヘッダーの高さ分
    padding-top: 65px !important;
    @include smartphone-horizontal {
        padding-top: 0px !important;
    }
    // ボトムナビゲーションバーの高さ分
    @include smartphone-vertical {
        padding-top: 0px !important;
        padding-bottom: calc(env(safe-area-inset-bottom) + 56px) !important;
    }
}

// ルートコンテナ
body .route-container {
    height: 100%;
    background: var(--v-background-base);
}

// ボタン内のテキストの字間をオフ
// 大文字への強制変換をオフ
.v-btn {
    letter-spacing: 0 !important;
    text-transform: none !important;
}

.v-snack {
    @include smartphone-vertical {
        padding-bottom: 56px !important;
    }
    .v-btn__content {
        color: var(--v-primary-lighten1);
        letter-spacing: 0.3;
    }
    .success, .info, .warning, .error {
        .v-btn__content {
            color: var(--v-text-base);
            letter-spacing: 0.3;
        }
    }
    &:not(.v-snack--absolute) {
        height: 100dvh !important;
    }
}

// ダイヤログ
.v-dialog {
    @include smartphone-vertical {
        margin: 24px 6px !important;
        &--fullscreen {
            margin: 0 !important;
        }
    }
}

// ツールチップのスタイル
.v-popper--theme-tooltip {
    .v-popper__inner {
        display: inline-block;
        padding: 4px 10px;
        border-radius: 4px;
        background: var(--v-background-lighten1);
        color: var(--v-text-base);
        font-size: 12px;
        font-family: 'Open Sans', 'YakuHanJPs', 'Hiragino Sans', 'Noto Sans JP', sans-serif;
        font-weight: 500;
        opacity: 0.9;
        line-height: 22px;
    }
    .v-popper__arrow-container {
        display: none;
    }
}

.v-tabs-items {
    background-color: transparent !important;
}

// タッチデバイスで hover を無効にする
// ref: https://github.com/vuetifyjs/vuetify/issues/2178
@media (hover: none) {
    :hover:before {
        background-color: transparent !important;
    }
}

// 選択時の色
*::selection {
	background-color: #E64F9780;
}

// 記号の装飾
.decorate-symbol {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    position: relative;
    padding: 0px 3px;
    margin-left: 2.5px;
    margin-right: 2.5px;
    border-radius: 4px;
    color: var(--v-text-base);
    background: var(--v-primary-base);
    font-size: 0.94em;
}

// スクロールバー
* {
    scrollbar-color: var(--v-gray-base) var(--v-background-base);
    scrollbar-width: thin;
}
::-webkit-scrollbar {
    width: 7px;
    height: 7px;
}
::-webkit-scrollbar-track {
    background: var(--v-background-base);
}
::-webkit-scrollbar-thumb {
    background: var(--v-background-lighten2);
    &:hover {
        background: var(--v-gray-base);
    }
}

.v-menu__content {
    @include smartphone-vertical {
        .v-list {
            background-color: var(--v-background-lighten1) !important;
        }
    }
    &::-webkit-scrollbar {
        width: 12px;
        height: 12px;
    }
    &::-webkit-scrollbar-thumb {
        border: solid 3.5px var(--v-background-base);
    }
}

.v-card__text {
    font-weight: inherit !important;
}

// ページ遷移アニメーション
.v-enter-active, .v-leave-active {
    transition: opacity 0.25s;
}
.v-enter, .v-leave-to {
    opacity: 0;
}
.v-enter-active.route-container {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
}

// カーソルをポインターにするやつ
.cursor-pointer {
    cursor: pointer;
}

// ユーティリティ
.w-25 {
    width: 25%;
}
.w-50 {
    width: 50%;
}
.w-75 {
    width: 75%;
}
.w-100 {
    width: 100%;
}
.h-25 {
    height: 25%;
}
.h-50 {
    height: 50%;
}
.h-75 {
    height: 75%;
}
.h-100 {
    height: 100%;
}

</style>