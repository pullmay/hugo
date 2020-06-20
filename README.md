# Hugo

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

    * 次のように編集

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