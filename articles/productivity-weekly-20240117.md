---
title: "OpenTofu正式リリース！GCPやレガシーコード改善も｜Productivity Weekly (2024-01-17号)"
emoji: "🎞️"
type: "idea"
topics: ["ProductivityWeekly", "生産性向上"]
published: false
publication_name: "cybozu_ept"
user_defined: {"publish_link": "https://zenn.dev/korosuke613/articles/productivity-weekly-20240117"}
---

こんにちは。サイボウズ株式会社 [生産性向上チーム](https://note.com/cybozu_dev/n/n1c1b44bf72f6)の平木場です。

僕たち生産性向上チームは毎週水曜日に Productivity Weekly という「1 週間の間に発見された開発者の生産性向上に関するネタを共有する会」を社内で開催しています。
本記事はその時のネタをまとめたものです。


2023-01-25 号から、基本的に隔週で連載することとしました。たまに単独でも投稿するかもしれません。
今週は 2024-01-17 単独号です。

今回が第 139 回目です。過去の記事は[こちら](https://zenn.dev/topics/productivityweekly?order=latest)。

:::message
2023-05-10 号から、実験的に生産性向上チームの他メンバーにいくつかのトピックを紹介していただくことにしています。

対象のトピックでは、文章の最後に `本項の執筆者: <執筆者名>` を追加しています。

今週の共同著者は次の方です。
- [@korosuke613](https://zenn.dev/korosuke613)
<!-- - [@defaultcf](https://zenn.dev/defaultcf) -->
- [@Kesin11](https://zenn.dev/kesin11)
- [@r4mimu](https://zenn.dev/r4mimu)
- [@uta8a](https://zenn.dev/uta8a)

:::

# news 📺

## OpenTofu Announces General Availability
https://www.linuxfoundation.org/press/opentofu-announces-general-availability

https://opentofu.org/blog/opentofu-is-going-ga/

HashiCorp 社のプロダクト Terraform をフォークした OSS、OpenTofu がついに正式リリースされました 🎉

半年前に HashiCorp が Terraform などの主要 OSS プロダクトを BUSL に変更する発表をして以来、OpenTofu を取り巻くさまざまな動きがありました。僕もだんだんいつ何があったか怪しくなってきたので雑に時系列でトピックをまとめました。（日付は時差とか考慮してない）

- 2023/08/10 [HashiCorp 社が主要 OSS 製品のライセンスを BUSL に変更すると発表](https://zenn.dev/cybozu_ept/articles/productivity-weekly-20230816#hashicorp-adopts-business-source-license)
- 2023/08/16 [OpenTF Foundation 爆誕。HashiCorp 社に対して OSS を継続するよう要求](https://zenn.dev/cybozu_ept/articles/productivity-weekly-20230816#opentf-foundation-%E7%88%86%E8%AA%95)
- 2023/08/25 BUSL になる前の Terraform からフォークした OSS 版 Terraform こと OpenTF の開発開始を OpenTF Foundation が発表
- 2023/09/05 [OpenTF のリポジトリが Public になる](https://zenn.dev/cybozu_ept/articles/productivity-weekly-20230913#the-opentf-fork-is-now-available!)
- 2023/09/20 [OpenTF Foundation が OpenTofu Foundation に改名、OSS としての OpenTF も OpenTofu に](https://github.com/opentofu/opentofu/issues/296)
- 2023/09/20 [OpenTofu が Linux Foundation 傘下に](https://www.linuxfoundation.org/press/announcing-opentofu)
- 2023/12/19 [OpenTofu のリリース候補（RC）がリリース](https://opentofu.org/blog/opentofu-release-candidate-is-out/)
- 2024/01/10 OpenTofu が正式リリース（GA）🎉
- 2024/03/19 CNCF[^cncf] 主催の KubeCon + CloudNativeCon Europe 2024 で OpenTofu Day を開催予定

なお、OpenTofu の意義や目指すところ、さまざまな経緯は minamijoyo さんの記事が大変参考になります。僕はこれを読む前は OpenTofu にそこまで前向きではなかったのですが、読んでみると OpenTofu の必要性が見え、ワクワクする未来を感じて考えを改めました。

- [Terraform職人のためのOpenTofu入門 #Terraform - Qiita](https://qiita.com/minamijoyo/items/16d1b5b15a60d17e350a#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)

Terraform v1.7 から BUSL に変更されているため、Terraform 最後の MPL 2.0 は v1.6 となります。そのため、OpenTofu は MPL 2.0 ライセンスで v1.6 から GA となっています。

Linux Foundation の記事ではコミュニティ主導のアプローチ＆オープンソースの価値が強調されています。OpenTofu 1.6 では早く・安定したリリースを可能にすることを目的にしているとのことですが、OpenTofu 1.7 では Terraform で利用できない、かつ、コミュニティで求められてきた多くの機能を導入する予定とのことです。
さらに詳しい内容は冒頭 2 つ目のリンクである OpenTofu のブログ記事をご覧ください。

GA とはなりましたがまだまだ OpenTofu を Terraform の代わりに使っていけるかは知見が少ない気はします。ぜひ試してみてバグ報告をやっていきましょう[^saikin]。

[^cncf]: Cloud Native Computing Foundation の略。クラウドネイティブを推進するために色々やってるでかいコミュニティで Linux Foundation 傘下（雑）。
[^saikin]: ちなみに僕はここ数ヶ月 kintone 開発チームで新機能開発を体験しているので、3 ヶ月くらい Terraform を触ってないです。生産性向上チームに戻ったら OpenTofu に置き換えて色々試したい。

_本項の執筆者: [@korosuke613](https://zenn.dev/korosuke613)_

## Our move to generated SDKs - The GitHub Blog
https://github.blog/2024-01-03-our-move-to-generated-sdks/

OpenAPI の定義から [Kiota](https://github.com/microsoft/kiota) を用いて自動生成された GitHub の [Go](https://github.com/octokit/go-sdk) と [.NET](https://github.com/octokit/dotnet-sdk) の SDK が公開されました（現在はまだ unstable）。

自分も今まで Kiota の名前は聞いたことがなかったのですが、Microsoft が OSS で開発している OpenAPI の定義ファイルから様々な言語のコードを生成するジェネレータのようです。興味がある方は [Kiota のドキュメント](https://learn.microsoft.com/ja-jp/openapi/kiota/)を見てみると雰囲気が掴めるかもしれません。自分は QUICKSTART で自分が使えるいくつかの言語のコードを見てみましたが、それぞれの言語ごとに API クライアントとして違和感のない使い方ができる印象でした。

今回の GitHub の記事では、OpenAPI から SDK を生成することで REST API のカバレッジをほぼ 100%にでき、今後は SDK をさらに良くすることに注力できると述べられています。
今のところ Go と.NET にだけしか言及されておらず、他の言語の SDK が今後どうなるのかなどは全く触れられていませんでした。Go と.NET もそれぞれのリポジトリの README にはまだ"alpha"バージョンや stable ではないと書かれているので、それらも含めて今後に期待ですね。


_本項の執筆者: [@Kesin11](https://zenn.dev/kesin11)_

# know-how 🎓

## GitHub Actionsのcomposite actionを使ってinternalリポジトリのファイルを配布する - Cybozu Inside Out | サイボウズエンジニアのブログ
https://blog.cybozu.io/entry/2024/01/11/000000

composite action を利用することで本来はデフォルトの `GITHUB_TOKEN` ではアクセスできない Internal リポジトリのファイルを配布する方法を紹介されている記事です。

GitHub Actions では通常デフォルトの `GITHUB_TOKEN` の権限で `git clone` を行うため、他のリポジトリも Public リポジトリであれば clone できますが、Private リポジトリを clone できません。ここまでは通常の感覚通りですが、GitHub Enterprise Cloud の場合はさらに Internal リポジトリという種類もあり、これは同じ Enterprise に所属するユーザーであれば誰でも READ 権限を持っています。いわば Enterprise 限定の Public と言えるため、例えば会社の全員に利用してもらいたいようなソースコードの置き場として便利なのですが、実は GitHub Actions のデフォルトの `GITHUB_TOKEN` では Private 同様にアクセスはできません。

この記事では `GITHUB_TOKEN` を使用する以外の方法の検討もしつつ、最終的に共有したいリポジトリを composite action として呼び出せるようにすることで他のリポジトリの GitHub Actions から `GITHUB_TOKEN` だけで必要なファイルにアクセスを可能にする方法を紹介しています。トリッキーな方法だとは思いますが、特にリポジトリを Public にしにくい業務用途では便利な場面がありそうです。

_本項の執筆者: [@Kesin11](https://zenn.dev/kesin11)_

## AWSコンテナ系アーキテクチャの選択肢を最適化する | 外道父の匠
https://blog.father.gedow.net/2024/01/12/aws-container-architecture-watershed/

AWS コンテナサービスに関するアーキテクチャ選択の最適化について解説している記事です。
インスタンスとコンテナ、ECS と EKS、EC2 上の ECS/EKS と Fargate という基盤選択の大きめなところから、ロードバランサーやデプロイ戦略、ログと監視などの運用上のトピックまで幅広く解説しています。

記事内では筆者の方が過去に調査した結果やリファレンスをもとに、意見が述べられており、説得力がある内容です。
個人的には [Fargate の CPU 性能と特徴についての記事](https://blog.father.gedow.net/2023/10/02/aws-ecs-fargate-cpu-2023/)が各 CPU の特徴を比較と整理されていてわかりやすかったし学びでした。これは Arm 一択ですね。
記事内のリファレンスもすべて目を通したら、書籍並みのボリュームになりそうですが、それだけ読む価値があると思います。(自分も全ては読めてないので、少しずつ読み進めます...！)

タイトルには AWS とありますが、クラウドネイティブなアーキテクチャの選択について考える上で、AWS 以外のクラウドでも参考になると思います！

_本項の執筆者: [@r4mimu](https://zenn.dev/r4mimu)_

## AWS側の目線から理解する、Google Cloud ロードバランサの世界 - How elegant the tech world is...!
https://iselegant.hatenablog.com/entry/google-cloud-load-balancer

Google Cloud のロードバランサーの種類と特徴について解説している記事です。
Google Cloud のロードバランサーは AWS のロードバランサーと比べて種類が多く、一見複雑です。
そこでまず、 Google Cloud のネットワークについての思想を確認し、ネットワーク設計の思想を踏まえたうえで、ステップ・バイ・ステップで各ロードバランサーの種類を解説しています。
各ステップ、図を提示しているので、ロードバランサーの種類を理解するのに役立ちます。

結局のところ、L7/L4 、内部/外部、グローバル/リージョン、プロキシ/パススルー といった区別が理解できれば、ロードバランサーの種類を理解できるということがわかりました。
記事の最後の図がわかりやすいので、そこだけでも見てみる価値があります。

自分自身、これまでは主に AWS を使っていましたが Google Cloud を使う場面がありました。
その際、AWS と異なり、VPC がグローバルリソースという所から、ロードバランサー含めネットワーク周りの理解が難しかったのですが、こちらの記事のおかげで理解が深まりました。

_本項の執筆者: [@r4mimu](https://zenn.dev/r4mimu)_

## テストプロセスが自走するチーム体制をめざして QA が取り組んでいること - Techtouch Developers Blog
https://techtouch.hatenablog.jp/entry/qa_test_it_yourself

## 実録レガシーコード改善 / Working with Legacy Code: the True Record - Speaker Deck
https://speakerdeck.com/twada/working-with-legacy-code-the-true-record

初回：https://findy.connpass.com/event/304101/
再放送：https://findy.connpass.com/event/307431/

t-wada さんが過去に実際に経験された開発案件において、最初にどのようにテストコードを追加し、その後に立て続けにやってくる新規要件を TDD で実装していったかという内容でした。
今回の題材は Lambda + Alexa スキルでしたが、利用するインフラや SDK からどのように疎結合にしてテストコードを書いていくかの考え方は他の開発にも通ずるはずです。

個人的には多機能なテストフレームワークやツールを使うのではなく、コードの設計側を見直すことでシンプルにテストコードを書きやすくする今回のアプローチはとても同意できました。Productivity Weekly ではむしろそのようなツールを紹介する機会が多いですが、何でもそのようなツールで解決するのではなく基本に立ち戻ることも大事だと改めて感じました。

スライドだけでも十分な情報量があるのですが、自分は生放送で見ていたのでやはりスライドでは伝わりきれない部分もあると感じました。アーカイブ動画が公開されるかはまだ不明ですが、もし公開されたらぜひ動画で見てほしいと思います。


_本項の執筆者: [@Kesin11](https://zenn.dev/kesin11)_

## 雰囲気でbuildx/BuildKitを使っていたので調べました
https://zenn.dev/fraim/articles/98ad17f9ed140e

# tool 🔨

## eBPFを使った自動テストツール「Keploy」がすごい
https://zenn.dev/jambowrd/articles/3ee00f61c0b827

# read more 🍘
Productivity Weekly で出たネタを全て紹介したいけど紹介する体力が持たなかったネタを一言程度で書くコーナーです。

- **news 📺**
  - [Amazon ECS and AWS Fargate now integrate with Amazon EBS](https://aws.amazon.com/jp/about-aws/whats-new/2024/01/amazon-ecs-fargate-integrate-ebs/)
    - Amazon ECS、AWS Fargate で簡単に Amazon EBS ボリュームをアタッチできるようになりました
    - ストレージやデータを大量に利用するアプリケーションにおいて、コンテナとストレージを切り離せるのは嬉しそうです

_本項の執筆者: [@korosuke613](https://zenn.dev/korosuke613)_

# あとがき
今週号でした。今週号は個人的にも興味深いネタが多くて楽しかったです。最近なかなか探求時間を取れていないので、今回自分が書いたネタは一個だけでした。チームメンバが意欲的に紹介を書いてくれるのでめちゃ助かってます。チームワークを感じるねぇ...

サイボウズの生産性向上チームでは社内エンジニアの開発生産性を上げるための活動を行なっています。そんな生産性向上チームが気になる方は下のリンクをクリック！
https://speakerdeck.com/cybozuinsideout/engineering-productivity-team-recruitment-information

<!-- :::message すみません、今週もおまけはお休みです...:::-->

