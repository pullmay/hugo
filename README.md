# Hugo

* [HUGO](https://gohugo.io/)
* [Hugo Themes](https://themes.gohugo.io/)

## 手順

1. GitHubにリポジトリを作成
1. ターミナルで以下を実行する

    ```
    $ hugo new site hugo
    $ cd hugo
    $ git clone https://github.com/Track3/hermit.git themes/hermit
    ```

1. `config`の設定

    ```
    $ vim config.toml
    ```

    * 次のように記述する

    ```
    baseURL = "https://pullmay.github.io/hugo"
    publishDir = "docs"
    languageCode = "ja-jp"
    title = "title"
    theme = "hermit"

    [params]
        dateform        = "Jan 2, 2006"
        dateformShort   = "Jan 2"
        dateformNum     = "2006-01-02"
        dateformNumTime = "2006-01-02 15:04 -0700"
    ```

1. 新しい記事の作成

    ```
    $ hugo new posts/first-content.md
    $ vim /content/posts/first-content.md
    ```

    * `first-content.md`において`draft: true`を`draft: false`に変更

1. 以下を実行

    ```
    $ git remote add https://github.com/pullmay/hugo.git 
    $ git merge --allow-unrelated-histories origin/master
    ```

1. `deploy.sh`を作成する

    ```
    $ vim deploy.sh
    ```

    * 次のように記述する

    ```
    #!/bin/bash

    echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

    # Build the project.
    hugo -t hermit

    # Go To Public folder
    cd docs
    # Add changes to git.
    git add .

    # Commit changes.
    msg="Update"
    if [ $# -eq 1 ]
    then msg="$1"
    fi
    git commit -m "$msg"

    # Push source and build repos.
    git push origin master

    # Come Back up to the Project Root
    cd ..

    # Commit source repository changes
    git add .
    git commit -m "$msg"
    git push
    ```

1. `deploy.sh`を実行する

    ```
    $ ./deploy.sh   
    ```

## GitHubの設定

* Setting -> GitHub Pages -> Source -> master branch / docs folder

## Reference

* [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
* [Hermit](https://github.com/Track3/hermit)
* [Using HUGO](https://fukasawah.github.io/posts/2018/12/24/using-hugo/)
* [chanmitsu55blog](https://chanmitsu55.github.io/2017/12/25/2017-12-25-create-blog-by-hugo/)
* [さくらのナレッジ](https://knowledge.sakura.ad.jp/22908/)
* [パソコン工房](https://www.pc-koubou.jp/magazine/30737)

## Debug

* [config.toml](https://github.com/Track3/hermit/blob/master/exampleSite/config.toml#L28-L32)
* [Qiita](https://qiita.com/1915keke/items/b858a9eef5ddfaa338dc)

## 追記

* tweetの埋め込みは次のようにする

```
{{< tweet 1273825702970748929 >}}
```

* デフォルトで用意されていないものについては`layouts`フォルダの下に`shortcodes`というフォルダを作ってその中に`html`ファイルを置く（Ref:[spf13](https://github.com/spf13/spf13.com/tree/master/layouts/shortcodes)）．

* `hugo new post/first-content.md`とすると記事となる`md`ファイルが生成される．他にディレクトリを作りたい場合は，`hugo nrew another-folder/another-file.md`とすればいいわけだが，これだと少し不都合なことが生じるようだ．具体的には`tags`で設定したタグが表示されなかったり，前後のページに遷移するボタンがなかったりする．これを何とかするためには，`./themes/hermit/layouts`に移動して，新しく作成したフォルダと同名のディレクトリを作る．今回の場合は，`mkdir another-folder`とする．そして，元々あるであろう`posts`というフォルダの中に入っているファイルをコピーする．例えば，`cp posts/* ./another-folder/`とでもすればよい．こうしてあげると，設定が反映される．

* `footer`の部分が正常に表示されない件に関して，とりあえずこうしたらうまくいったという話を書いておく．まず，`themes/hermit/layouts`の中の`posts`というフォルダがあることを確認する．このフォルダの中にある`single.html`には，`content`フォルダの中の`posts`に含まれるファイルに対して適用される設定について記載されている．`single.html`を開き，`40`行目付近にある以下の部分を以下のように変更する．正直，`themes/hermit`の中身を自分で変更を加えることはよくないと思うのだが，他に方法を思い付かなかったので，ひとまずこれで対応したい．

    * 変更前

    ```html
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>{{ i18n "wordCount" . }}</p>
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>{{ dateFormat .Site.Params.dateformNumTime .Date.Local }}</p>
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>{{ dateFormat .Site.Params.dateformNumTime .Lastmod.Local }}</p>
    ```

    * 変更後

    ```html
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>{{ .WordCount }} Words</p>
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>Created at: {{ dateFormat .Site.Params.dateformNumTime .Date.Local }}</p>
    <p><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>Updated at: {{ dateFormat .Site.Params.dateformNumTime .Lastmod.Local }}</p>
    ```

    * 記事のヘッダー部分の例

    ```markdown {hl_lines=[4]}
    ---
    title: "200620"
    date: 2020-06-20T15:49:17+09:00
    lastmod: 2020-06-20T22:49:17+09:00
    draft: false
    tags: 
    - TIL
    ---
    ```

## ToDo

- [ ] メイン画面の作成手順
- [ ] `config.toml`のより詳細な記述
- [ ] 他のページ（`About`ページなど）の追加