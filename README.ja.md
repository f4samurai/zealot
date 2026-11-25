<div align='center'>
  <a href="https://www.producthunt.com/posts/zealot?utm_source=badge-featured&utm_medium=badge&utm_souce=badge-zealot" target="_blank"><img src="https://api.producthunt.com/widgets/embed-image/v1/featured.svg?post_id=322207&theme=light" style="width: 250px; height: 54px" width="250" height="54" /></a>

  <h1>Zealot</h1>

  <h4>
    アプリに関わる作業をまるごと継続的に回す、オープンソースのセルフホスト型サービスです。どの CI とも連携して自動化でき、企業での長年の運用経験をもとに、ビルドのアップロードから配布までの流れを自前の環境で提供します。En Taro Adun! 🖖
  </h4>

  <a href="https://github.com/tryzealot/zealot/blob/develop/CHANGELOG.md">
    <img alt="changelog" src="https://img.shields.io/github/v/release/tryzealot/zealot?include_prereleases">
  </a>
  <a href="https://ghcr.io/tryzealot/zealot">
    <img alt="docker image" src="https://img.shields.io/docker/pulls/tryzealot/zealot.svg">
  </a>
  <a href="https://t.me/+csa3Y2KOx44wMGRl">
    <img alt="chat on telegram" src="https://img.shields.io/badge/chat-on%20telegram-important.svg">
  </a>
  <a title="Crowdin" target="_blank" href="https://crowdin.com/project/zealot"><img src="https://badges.crowdin.net/zealot/localized.svg"></a>
  <a href="https://codeclimate.com/github/tryzealot/zealot/maintainability">
    <img alt="codeclimate" src="https://api.codeclimate.com/v1/badges/f79b2fed0ce166b2ea2c/maintainability" />
  </a>
  <a href="https://www.codacy.com/gh/tryzealot/zealot/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=tryzealot/zealot&amp;utm_campaign=Badge_Grade">
    <img alt="codacy" src="https://app.codacy.com/project/badge/Grade/5e5c7bbeb1214fa39b11a7414f0d7171"/>
  </a>

  <div>
    <a href="https://zealot.ews.im/docs/self-hosted">インストール</a> •
    <a href="https://zealot.ews.im/docs/developer-guide/api">REST API</a> •
    <a href="https://zealot.ews.im/docs/developer-guide">開発者ガイド</a> •
    <a href="https://zealot.ews.im/docs/user-guide">ユーザーガイド</a>
  </div>

  <sub>公式ドキュメントは英語版と中国語版だけです。上のリンクは英語版です。</sub>
</div>

![Zealot Showcase](https://github.com/tryzealot/docs/blob/main/static/img/showcase-light.png#gh-light-mode-only)
![Zealot Showcase](https://github.com/tryzealot/docs/blob/main/static/img/showcase-dark.png#gh-dark-mode-only)

## このリポジトリについて

[tryzealot/zealot](https://github.com/tryzealot/zealot) の fork です。テスト用アプリの配布に使うため、upstream に次の変更を加えています。
既定のブランチは `develop` で、upstream の `develop` にこの変更を足したものです。

| 変更 | PR |
|---|---|
| 本番環境（production）でも `ZEALOT_USE_HTTPS=false` で HTTP のまま運用できるようにした。未設定なら upstream と同じ | [#1](https://github.com/f4samurai/zealot/pull/1) |
| 新規ユーザーの言語・タイムゾーンを、サイトの既定（`DEFAULT_LOCALE` / `TIME_ZONE`）に従わせた | [#1](https://github.com/f4samurai/zealot/pull/1) |
| Webhook の送信に失敗したときの再送が Rails 8.1 で動かなかったのを直した | [#2](https://github.com/f4samurai/zealot/pull/2) |
| ビルドのダウンロードに、ログインか API token を必須にした。`/uploads/` と `/backup/` への直接のアクセスは 404 にした（アプリのアイコンは除く） | [#3](https://github.com/f4samurai/zealot/pull/3) |
| Webhook の本文で `{{release_id}}` を使えるようにした | [#4](https://github.com/f4samurai/zealot/pull/4) |

### 画面を日本語にする

画面の日本語訳は upstream にあります。環境変数で既定の言語とタイムゾーンを指定します。

```sh
DEFAULT_LOCALE=ja
TIME_ZONE=Asia/Tokyo
```

ユーザーごとの言語は、各ユーザーの設定画面で変えられます。

### スクリプトからビルドをダウンロードする

この fork では、未ログインのままビルドをダウンロードできません（#3）。スクリプトからは API token をヘッダーで渡します。
API token は、ログインしてユーザー設定の画面を開くと「API トークン」の欄にあります。

```sh
curl -fL -H "Authorization: Token <API token>" -o app.apk "https://<Zealot のホスト>/download/releases/<release の id>"
```

- `Authorization: Bearer <API token>` と `X-Zealot-Token: <API token>` でも通ります
- ダウンロード URL のクエリ（`?token=`）では受け付けません。URL はログやチャットに貼られることが多く、token ごと漏れるためです
- パスワードを設定したチャンネルは、従来どおりパスワードでもダウンロードできます

### upstream の取り込み

upstream の `develop` を `develop` にマージします。`README.md` は upstream のまま変えず、この fork の説明はこのファイル（`README.ja.md`）に書きます。マージのたびに `README.md` が衝突しないようにするためです。

## 特徴

- 🌏 **さまざまなプラットフォームのアプリを管理**: macOS・iOS・Android（apk / aab）・Windows・Linux に対応
- 📱 **テスト端末をまとめて管理**: iOS のテスト端末の情報を自動で同期し、新しい端末をワンクリックで Apple Developer に登録できる
- 🧑‍💻 **開発者向けのツールが揃っている**: REST API、[iOS][zealot-ios-sdk] と [Android][android-android-sdk] の SDK、自動ビルド用の [fastlane プラグイン][fastlane-plugin-zealot]
- 💥 **アプリの中身を解析**: iOS・Android のアプリや iOS のプロビジョニングプロファイルのメタ情報を読み取る
- 🚨 **イベントを通知**: 本文を自由に組める Webhook で、任意の通知サービスに送れる
- 🗄 **チャンネルで分けて管理**: 用途や製品の形態ごとに、アプリのチャンネルを分けて管理できる
- 🎳 **さまざまな環境にデプロイ**: amd64 / arm64 に対応し、デプロイの方法もいろいろ選べる
- 🔑 **外部アカウントでログイン**: Feishu（Lark）・GitLab・GitHub・Google・LDAP・OIDC
- 🌑 **ダークモード**: ライトとダークを自由に切り替えられる

## デモ

- デモサイト: https://tryzealot.ews.im
- ログイン用のアカウントは [README.md のデモの節](README.md#在线演示) にあります（英語画面のアカウントは `English Account` の行）

> **注意**: デモサイトのデータは毎日初期化されます。アップロードしたアプリについて、運営者は一切の責任を負いません。自己責任で使ってください。

## 寄付

<table>
  <tr>
    <td align="center">
      <a href="https://afdian.com/a/icyleaf">
        <img src="https://afdian.com/static/img/icons/android-chrome-192x192.png" height="46" width="46" alt="afdian" title="afdian で寄付する" />
      </a><br />
      <sub>afdian で寄付</sub>
    </td>
    <td align="center">
      <a href="https://github.com/sponsors/icyleaf">
        <img src="https://camo.githubusercontent.com/369f5e3f574fd9693f5b1b6e193eb5211827ccacd0eec91ede06f7574e20f0c7/68747470733a2f2f6769746875622e6769746875626173736574732e636f6d2f696d616765732f6d6f64756c65732f736974652f73706f6e736f72732f6c6f676f2d6d6f6e612e737667" width="46" height="46" alt="Github Sponsor" title="Become a Github Sponsor" />
      </a><br />
      <sub>Become A Sponsor</sub>
    </td>
    <td align="center">
      <a href="https://buymeacoffee.com/icyleaf">
        <img src="https://studio.buymeacoffee.com/assets/img/bmc-meta-new/new/apple-icon-120x120.png" height="46" width="46" alt="Buy Me A Coffee" title="Buy Me A Coffee" />
      </a><br />
      <sub>Buy Me A Beer</sub>
    </td>
  </tr>
</table>

## 開発の統計

![Alt](https://repobeats.axiom.co/api/embed/caba5e356c0e8258d395aaa9f70fec475a2eb643.svg "Repobeats analytics image")

## ライセンス

[MIT][mit-link]

[zealot-ios-sdk]: https://github.com/tryzealot/zealot-ios
[android-android-sdk]: https://github.com/tryzealot/zealot-android
[fastlane-plugin-zealot]: https://github.com/tryzealot/fastlane-plugin-zealot
[mit-link]: https://github.com/tryzealot/zealot/blob/develop/LICENSE
