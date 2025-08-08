# JabRef Browser Extension

> [Firefox](https://addons.mozilla.org/en-US/firefox/addon/jabref/?src=external-github) -  [Chrome](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh) - [Edge](https://microsoftedge.microsoft.com/addons/detail/pgkajmkfgbehiomipedjhoddkejohfna) - [Vivaldi](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh)

Browser extension for users of the bibliographic reference manager [JabRef](https://www.jabref.org/).
It automatically identifies and extracts bibliographic information on websites and sends them to JabRef with one click.

When you find an interesting article through Google Scholar, the arXiv or journal websites, this browser extension allows you to add those references to JabRef.
Even links to accompanying PDFs are sent to JabRef, where those documents can easily be downloaded, renamed and placed in the correct folder.
[A wide range of publisher sites, library catalogs and databases are supported](https://www.zotero.org/support/translators).

_Please post any issues or suggestions [here on GitHub](https://github.com/JabRef/JabRef-Browser-Extension/issues)._

## Installation and Configuration

Normally, you simply install the extension from the browser store and are ready to go.
> [Firefox](https://addons.mozilla.org/en-US/firefox/addon/jabref/?src=external-github) -  [Chrome](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh) - [Edge](https://microsoftedge.microsoft.com/addons/detail/pgkajmkfgbehiomipedjhoddkejohfna) - [Vivaldi](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh)

Sometimes, a manual installation is necessary (e.g. if you use the portable version of JabRef). In this case, please follow the steps described [in the user manual](https://docs.jabref.org/import-export/import/jabref-browser-extension).

## Usage

After the installation, you should be able to import bibliographic references into JabRef directly from your browser.
Just visit a publisher site or some other website containing bibliographic information (for example, [the arXiv](http://arxiv.org/list/gr-qc/pastweek?skip=0&show=5)) and click the JabRef symbol in the Firefox search bar (or press `Alt` + `Shift` + `J`).
Once the JabRef browser extension has extracted the references and downloaded the associated PDF's, the import window of JabRef opens.

You might want to configure JabRef so that new entries are always imported in an already opened instance of JabRef.
For this, activate "Remote operation" under the Network tab in the JabRef Preferences.

## About this Add-On

Internally, this browser extension uses the magic of Zotero's site translators.
Thus most of the credit has to go to the Zotero development team and to the many authors of the [site translators collection](https://github.com/zotero/translators).
Note that this browser extension does not make any changes to the Zotero database and thus both plug-ins coexist happily with each other.

## Contributing to the Development

JabRef browser extension uses the [WebExtensions API](https://developer.mozilla.org/en-US/Add-ons/WebExtensions).

Preparation:

1. Install [Node.js](https://nodejs.org) (e.g., `choco install nodejs`)
2. Install [gulp](https://gulpjs.com/) and [web-ext](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Getting_started_with_web-ext): `npm install --global gulp-cli web-ext`
3. [Fork the repository](https://help.github.com/articles/fork-a-repo/).
4. Checkout the repository.
5. Install development dependencies via `npm install`.
6. Start browser with the add-on activated:
   Firefox: `npm run dev:firefox`
   Chrome: `npm run dev:opera`

Now just follow the typical steps to [contribute code](https://guides.github.com/activities/contributing-to-open-source/#contributing):

1. Create your feature branch: `git checkout -b my-new-feature`
2. Build and run the add-on as described above.
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request.

To update dependencies:

- `npm outdated` gives an overview of outdated packages ([doc](https://docs.npmjs.com/cli/outdated))
- `npm-upgrade` updates all packages
- `npm install` install updated packages
- running

  ```sh
   git subtree pull --prefix zotero-connectors https://github.com/zotero/zotero-connectors.git master --squash
   git subtree pull --prefix zotero-connectors/src/zotero https://github.com/zotero/zotero.git master --squash
   git subtree pull --prefix zotero-connectors/src/translate https://github.com/zotero/translate.git master --squash
   git subtree pull --prefix zotero-connectors/src/utilities https://github.com/zotero/utilities.git master --squash
   git subtree pull --prefix zotero-scholar-citations https://github.com/MaxKuehn/zotero-scholar-citations.git master --squash
  ```

  updates the `zotero-connectors` submodule and the `zotero-scholar-citations` submodule  

- `gulp update-external-scripts` copies and post-processes the scripts in the folders `zotero-connectors` and `zotero-scholar-citations` to the folder `external-scripts`

## Release of new version

- Increase version number in `manifest.json`
- `npm run build`
- Upload to:
  - <https://addons.mozilla.org/en-US/developers/addon/jabref/versions/submit/>
  - <https://chrome.google.com/u/2/webstore/devconsole/26c4c347-9aa1-48d8-8a22-1c79fd3a597e/bifehkofibaamoeaopjglfkddgkijdlh/edit/package>
  - <https://addons.opera.com/developer/upload/>
  - <https://developer.apple.com/app-store-connect/>
- Remove the `key` field in `manifest.json` and build again. Then upload to:
  - <https://partner.microsoft.com/en-us/dashboard/microsoftedge/2045cdc1-808f-43c4-8091-43e2dcaff53d/packages>

## ビルド・更新

zotero-connectorsを更新することでメタデータを取得できるサイトを増やしました。

認識できるサイトは増えたけど正しく動いているかどうか分からないのと、**脆弱性が80個（！？）**あるので注意。
`78 vulnerabilities (3 low, 22 moderate, 22 high, 31 critical)`

・・・まあ脆弱性は公式のプラグインにもあるけどね。

実際のところ、`vivaldi://extensions`から`サイトへのアクセス`をクリックされた場合のみに変更すれば問題はないと思う。
そこ抜かれるならもうChromeの脆弱性だし。

```sh
# この手順はオリジナルのビルド方法です。
# macでビルドするとcanvasのインストールで詰まったのでWindowsがおすすめです。
npm install --global gulp-cli web-ext
# このリポジトリを使うなら`git submodule update --remote --recursive`した後にcd zotero-connectorsでOK
git clone git@github.com:JabRef/JabRef-Browser-Extension.git
cd JabRef-Browser-Extension
rm -fr zotero-connectors
#  git subtree pull --prefix zotero-connectors https://github.com/zotero/zotero-connectors.git master --squashするとCONFLICTするのでだめ
git clone --recursive https://github.com/zotero/zotero-connectors.git
# zotero-scholar-citationsも似たような感じで更新できると思います。使わないからやってないけど
cd zotero-connectors
# 注意：npm audit fixしたらぶっ壊れたので実行しないこと
npm install
cd ..

# package.jsonのconvasのバージョンを適当なものに変更します
# 今回は現時点での最新版の3.1.2に変更しました
code .
npm install
npm build
# npm run dev:operaするときはchromiumのあるパスを指定し直す必要がある

# 完成品のzipファイルを展開
code 展開したzipファイルの中身
# 1. manifest.jsonのdeveloperを削除（Unrecognized manifest key 'developer'と出る）
# 2. external-scripts/zotero.jsの376~の381のif-else文を削除（なんか知らんがエラー出る。消していいのかどうかは知らない。え？）
```

manifest v3にしようとしたけどよく分からん。
助けてくれ〜
