<template>
    <!-- ベース画面の中にそれぞれの設定画面で異なる部分を記述する -->
    <SettingsBase>
        <h2 class="settings__heading">
            <router-link v-ripple class="settings__back-button" to="/settings/">
                <Icon icon="fluent:arrow-left-12-filled" width="25px" />
            </router-link>
            <Icon icon="fa-solid:sliders-h" width="19px" style="margin: 0 4px;" />
            <span class="ml-3">全般</span>
        </h2>
        <div class="settings__content">
            <div class="settings__item">
                <div class="settings__item-heading">ピン留め中チャンネルの並び替え</div>
                <div class="settings__item-label">
                    ピン留め中のチャンネルの表示順序を変更できます。よくみるチャンネルは先頭に配置しておくと便利です。<br>
                    ピン留め中のチャンネルの追加・削除は、別途 TV ホーム画面のチャンネルリストから行えます。<br>
                </div>
            </div>
            <v-btn class="settings__save-button mt-4" depressed @click="pinned_channel_settings_modal = !pinned_channel_settings_modal">
                <Icon icon="iconamoon:sorting-left-bold" height="19px" />
                <span class="ml-1">ピン留め中チャンネルの並び替え設定を開く</span>
            </v-btn>
            <div class="settings__item settings__item--switch">
                <label class="settings__item-heading" for="tv_channel_selection_requires_alt_key">チャンネル選局のキーボードショートカットを {{Utils.AltOrOption()}} + 数字キー/テンキーに変更する</label>
                <label class="settings__item-label" for="tv_channel_selection_requires_alt_key">
                    この設定をオンにすると、数字キーまたはテンキーに対応するリモコン番号（1～12）のチャンネルに切り替える際、{{Utils.AltOrOption()}} キーを同時に押す必要があります。<br>
                    コメントやツイートを入力しようとして誤って数字キーを押してしまい、チャンネルが変わってしまう事態を避けたい方におすすめです。<br>
                </label>
                <v-switch class="settings__item-switch" id="tv_channel_selection_requires_alt_key" inset hide-details
                    v-model="settingsStore.settings.tv_channel_selection_requires_alt_key">
                </v-switch>
            </div>
            <v-divider class="mt-6"></v-divider>
            <div class="settings__item">
                <div class="settings__item-heading">デフォルトのパネルの表示状態</div>
                <div class="settings__item-label">
                    視聴画面を開いたときに、右側のパネルをどう表示するかを設定します。<br>
                </div>
                <v-select class="settings__item-form" outlined hide-details :dense="is_form_dense"
                    :items="panel_display_state" v-model="settingsStore.settings.panel_display_state">
                </v-select>
            </div>
            <div class="settings__item">
                <div class="settings__item-heading">テレビをみるときにデフォルトで表示されるパネルのタブ</div>
                <div class="settings__item-label">
                    テレビの視聴画面を開いたときに、右側のパネルで最初に表示されるタブを設定します。<br>
                </div>
                <v-select class="settings__item-form" outlined hide-details :dense="is_form_dense"
                    :items="tv_panel_active_tab" v-model="settingsStore.settings.tv_panel_active_tab">
                </v-select>
            </div>
            <div class="settings__item">
                <div class="settings__item-heading">ビデオをみるときにデフォルトで表示されるパネルのタブ</div>
                <div class="settings__item-label">
                    ビデオの視聴画面を開いたときに、右側のパネルで最初に表示されるタブを設定します。<br>
                </div>
                <v-select class="settings__item-form" outlined hide-details :dense="is_form_dense"
                    :items="video_panel_active_tab" v-model="settingsStore.settings.video_panel_active_tab">
                </v-select>
            </div>
            <v-divider class="mt-6"></v-divider>
            <div class="settings__item">
                <div class="settings__item-heading">設定をエクスポート</div>
                <div class="settings__item-label">
                    このデバイス（ブラウザ）に保存されている設定データを、エクスポート（ダウンロード）できます。<br>
                    ダウンロードした設定データ (KonomiTV-Settings.json) は、[設定をインポート] からインポートできます。異なるサーバーの KonomiTV を同じ設定で使いたいときなどに使ってください。<br>
                </div>
            </div>
            <v-btn class="settings__save-button mt-4" depressed @click="exportSettings()">
                <Icon icon="fa6-solid:download" class="mr-3" height="19px" />設定をエクスポート
            </v-btn>
            <div class="settings__item">
                <div class="settings__item-heading error--text text--lighten-1">設定をインポート</div>
                <div class="settings__item-label">
                    [設定をエクスポート] でダウンロードした設定データを、このデバイス（ブラウザ）にインポートできます。<br>
                    設定をインポートすると、<b>現在のデバイス設定はすべて上書きされます。</b>元に戻すことはできません。<br>
                    設定のデバイス間同期がオンのときは、<b>同期が有効なすべてのデバイスに反映されます。</b>十分ご注意ください。<br>
                </div>
                <v-file-input class="settings__item-form" outlined hide-details placeholder="設定データ (KonomiTV-Settings.json) を選択"
                    :dense="is_form_dense"
                    accept="application/json"
                    prepend-icon=""
                    prepend-inner-icon="mdi-paperclip"
                    v-model="import_settings_file">
                </v-file-input>
            </div>
            <v-btn class="settings__save-button error mt-5" depressed @click="importSettings()">
                <Icon icon="fa6-solid:upload" class="mr-3" height="19px" />設定をインポート
            </v-btn>
            <div class="settings__item">
                <div class="settings__item-heading error--text text--lighten-1">設定を初期状態にリセット</div>
                <div class="settings__item-label">
                    このデバイス（ブラウザ）に保存されている設定データを、初期状態のデフォルト値にリセットできます。<br>
                    設定をリセットすると、元に戻すことはできません。<br>
                    設定のデバイス間同期がオンのときは、<b>同期が有効なすべてのデバイスに反映されます。</b>十分ご注意ください。<br>
                </div>
            </div>
            <v-btn class="settings__save-button error mt-5" depressed @click="resetSettings()">
                <Icon icon="material-symbols:device-reset-rounded" class="mr-2" height="23px" />設定をリセット
            </v-btn>
        </div>
        <PinnedChannelSettings :modelValue="pinned_channel_settings_modal" @update:modelValue="pinned_channel_settings_modal = $event" />
    </SettingsBase>
</template>
<script lang="ts">

import { mapStores } from 'pinia';
import { defineComponent } from 'vue';

import PinnedChannelSettings from '@/components/Settings/PinnedChannelSettings.vue';
import Message from '@/message';
import useSettingsStore from '@/stores/SettingsStore';
import Utils from '@/utils';
import SettingsBase from '@/views/Settings/Base.vue';

export default defineComponent({
    name: 'Settings-General',
    components: {
        PinnedChannelSettings,
        SettingsBase,
    },
    data() {
        return {

            // ユーティリティをテンプレートで使えるように
            Utils: Object.freeze(Utils),

            // フォームを小さくするかどうか
            is_form_dense: Utils.isSmartphoneHorizontal(),

            // ピン留め中チャンネルの並び替え設定のモーダルを表示するか
            pinned_channel_settings_modal: false,

            // デフォルトのパネルの表示状態の選択肢
            panel_display_state: [
                {text: '前回の状態を復元する', value: 'RestorePreviousState'},
                {text: '常に表示する', value: 'AlwaysDisplay'},
                {text: '常に折りたたむ', value: 'AlwaysFold'},
            ],

            // テレビをみるときにデフォルトで表示されるパネルのタブの選択肢
            tv_panel_active_tab: [
                {text: '番組情報タブ', value: 'Program'},
                {text: 'チャンネルタブ', value: 'Channel'},
                {text: 'コメントタブ', value: 'Comment'},
                {text: 'Twitter タブ', value: 'Twitter'},
            ],

            // ビデオをみるときにデフォルトで表示されるパネルのタブの選択肢
            video_panel_active_tab: [
                {text: '番組情報タブ', value: 'RecordedProgram'},
                {text: 'シリーズタブ', value: 'Series'},
                {text: 'コメントタブ', value: 'Comment'},
                {text: 'Twitter タブ', value: 'Twitter'},
            ],

            // 選択された設定データ (KonomiTV-Settings.json) が入る
            import_settings_file: null as File | null,
        };
    },
    computed: {
        ...mapStores(useSettingsStore),
    },
    methods: {

        // 設定データをエクスポートする
        exportSettings() {

            // 設定データを JSON 化して取得
            const settings_json = JSON.stringify(this.settingsStore.settings, null, 4);

            // ダウンロードさせるために一旦 Blob にしてから、KonomiTV-Settings.json としてダウンロード
            const settings_json_blob = new Blob([settings_json], {type: 'application/json'});
            Utils.downloadBlobData(settings_json_blob, 'KonomiTV-Settings.json');
            Message.success('設定をエクスポートしました。');
        },

        // 設定データをインポートする
        async importSettings() {

            // 設定データが選択されていないときは実行しない
            if (this.import_settings_file === null) {
                Message.error('インポートする設定データを選択してください！');
                return;
            }

            // 設定データのインポートを実行
            const result = await this.settingsStore.importClientSettings(this.import_settings_file);
            if (result === true) {
                Message.success('設定をインポートしました。');
                window.setTimeout(() => this.$router.go(0), 300);
            } else {
                Message.error('設定データが不正なため、インポートできませんでした。');
            }
        },

        // 設定データをリセットする
        async resetSettings() {
            await this.settingsStore.resetClientSettings();
            Message.success('設定をリセットしました。');
            window.setTimeout(() => this.$router.go(0), 300);
        },
    }
});

</script>