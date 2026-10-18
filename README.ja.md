# Linux の権限昇格ラボ：setuid バイナリ、sudo のルール、書き込み可能な cron ジョブ

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml)

授業でそのまま使える **Linux の権限昇格ラボ**です。学校や学校法人が、生徒一人ひとりに専用のマシンとブラウザー上のターミナルを用意できる [MitsuruTech ラーニングプラットフォーム](https://mitsurutech.co.uk/ja/learning)から提供しています。生徒はそれぞれ、専用の Ubuntu VM を使います。VM には、権限の低い 2 つ目のアカウント `intern` があらかじめ用意され、見つけてもらうための、管理者がよくやる 3 つのミスが仕込まれています。もともと付けるはずのない setuid ビットを持つヘルパープログラム、`intern` がパスワードなしでエディターを root として実行できる sudo のルール、そして root 自身の cron が毎分実行する、誰でも書き込めるメンテナンススクリプトです。課題は、このうちの 1 つを使って root になり、`/root/flag.txt` を読み、何を使ったかを書き上げることです。

これはセキュリティの演習なので、最初に守るべき唯一のルールを書きます。攻撃してよいのは自分の VM だけです。この VM には、ほかの生徒のマシンへの経路も、Ubuntu のパッケージアーカイブ、PyPI、npm、GitHub 以外のインターネットへの経路もありません。ここで学んだことを、自分のものではないマシンに決して向けないでください。英国の Computer Misuse Act 1990 の下では、許可なくそうすることは刑事犯罪です。

## ラボ

| ファイル | 生徒がすること | ヘルプページ |
| --- | --- | --- |
| [`linux-privesc.lab.yaml`](linux-privesc.lab.yaml) | 権限の低いアカウント `intern` に切り替え、仕込まれた 3 つの弱点のうち 1 つを見つけ、それを使って root になり、そのことを証明する | [Linux 権限昇格ラボ](https://mitsurutech.co.uk/ja/help/lab-linux-privesc) |

セットアップは、生徒のホームフォルダーに README を置きます。そこには、ルール、2 つ目のアカウントの名前とパスワード、そして root が入り口を残していることが書かれています。攻略の手順は書かれていません。

**採点スクリプトはありません。意図的です。**VM 上の生徒自身のアカウントには管理者権限があるため、スクリプトが証拠として読めるファイルは、3 つの弱点を使わなくても、生徒が 1 行で書けてしまいます。実際に試したところ、チェックを通りました。簡単に偽造できる採点は、ないよりも悪いので、課題は短い文章での報告にしています。どの弱点を使い、どのコマンドを実行し、フラグに何と書かれていたかを、教員が読みます。そのあとで弱点を修正することは求めていませんが、教員が追加の課題として出すことはできます。

## ファイルの中身

`linux-privesc.lab.yaml` は、プラットフォームがそのまま実行する、**VM ラボの定義**一式です。

| キー | 役割 |
| --- | --- |
| `cpus`、`memory`、`disk` | 生徒の VM の大きさ：CPU 1 つ、1024 MB、6 GB |
| `open` | ブラウザーで開くポート：なし。ラボはターミナルです |
| `setup` | VM の初回起動時に root として一度だけ実行されるスクリプト。`intern` アカウントを作り、フラグを書き込み、3 つの弱点を仕込み、README を書きます |

## プラットフォームで使う

**クラスの課題として出すには：**

1. モジュールで課題を追加し、**Ubuntu VM ラボ**のカードを選びます。
2. サンプルを押してそこから始めるか、`linux-privesc.lab.yaml` の内容を貼り付けます。サンプルを使うとテキストが置き換わりますが、元に戻すで自分のものに戻せます。
3. 公開し、まず自分のコピーで試します。生徒にはそれぞれ専用の VM が用意され、セッションをまたいで保持されます。
4. ほかの課題と同じように配布し、提出の設定でドキュメントを選んで、生徒が文章での報告を提出できるようにします。ほかの提出物と同じように読んでください。マシン上には採点するものがありません。

マニュアルの [Ubuntu VM ラボのガイド](https://mitsurutech.co.uk/ja/help/labs)と[サンプルラボのページ](https://mitsurutech.co.uk/ja/help/sample-labs)に、そのほかのことが載っています。学校のアカウントはまだありませんか？[最初の 1 年は無料](https://mitsurutech.co.uk/ja/pricing)で、生徒 50 人まで使えます。[登録](https://mitsurutech.co.uk/ja/signup)にはメールアドレスだけが必要です。

## ファイルの検証

push のたびに、`.github/workflows/check.yml` が、ファイルが上のキーを持つ有効な YAML であること、そして `setup` がプラットフォームの上限である 16 KB 以内であることを確認します。VM は起動しません。それは、プラットフォーム自身のマシンで行われます。

## ラボでできないこと

VM が届くのは Ubuntu のパッケージアーカイブ、PyPI、npm、GitHub だけなので、このラボの向こう側に攻撃する対象はなく、攻撃を仕掛ける 2 台目のマシンもありません。追加のアカウントを 1 つ持つ、VM 1 台だけです。

## この演習について

[Mitsuru Technologies](https://mitsurutech.co.uk) が、[MitsuruTech ラーニングプラットフォーム](https://mitsurutech.co.uk/ja/learning)のために作りました。学校向けに、Python、Java、Go、Node、SQL、Linux をブラウザーで使えるようにし、コンテナーラボ、VM ラボ、GitHub から公開する Web アプリ、自動採点されるクイズも備えています。アカウントなしでコードを試せる[プレイグラウンド](https://mitsurutech.co.uk/ja/playground)もあります。

同じ提供元のほかのラボ：[lab-netlab](https://github.com/mitsurutech/lab-netlab)、[lab-juice-shop](https://github.com/mitsurutech/lab-juice-shop)、[lab-dvwa](https://github.com/mitsurutech/lab-dvwa)、[lab-webgoat](https://github.com/mitsurutech/lab-webgoat)、[lab-mysql-phpmyadmin](https://github.com/mitsurutech/lab-mysql-phpmyadmin)、[lab-sample](https://github.com/mitsurutech/lab-sample)。

## ライセンス

この作品は [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) でライセンスされています。Mitsuru Technologies Ltd のクレジットを表示し、商用に使わず、これをもとに作ったものを同じライセンスで共有するなら、学習と授業のために共有・改変できます。全文は `LICENSE` ファイルにあります。

セットアップがインストールするもの、つまり `cron` と `vim` は、VM の起動時に Ubuntu 自身のアーカイブから入手され、それぞれのプロジェクトのライセンスに従います。どちらもここでは配布していません。
