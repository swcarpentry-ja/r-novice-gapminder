---
title: knitrを使ったレポート作成
teaching: 60
exercises: 15
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- 再現性のあるレポート作成の価値を理解する
- R Markdownファイルの基本構成を認識し、コンパイルする方法を学ぶ
- Rコードチャンクを理解し、その目的、構造、およびオプションを学ぶ
- 計算結果を議論する際に、テキストブロックにRの出力を埋め込むインラインチャンクの使用方法を実演する
- R Markdownファイルを他の形式にエクスポートする際の代替出力形式を把握する

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ソフトウェアとレポートをどのように統合できますか？

::::::::::::::::::::::::::::::::::::::::::::::::::



## データ解析レポート

データ解析者は通常、コラボレーター向けや将来の参照のために、自分の解析や結果を記述したレポートを多く作成します。

初心者の多くは、まず解析をすべて1つのRスクリプトに書き込み、スクリプトやさまざまなグラフを添付ファイルとしてメールで共有することから始めます。しかし、これは手間がかかり、どの添付ファイルがどの結果なのかを説明するのに長い議論を要する場合があります。

Wordや[LaTeX](https://www.latex-project.org/)を使って正式なレポートを書くと、このプロセスが簡単になります。解析レポートと出力グラフを1つの文書にまとめることができます。ただし、図の形式を整えたり、煩わしい改ページを修正したりするのに手間がかかり、一度の形式変更で新たな問題が発生する「モグラ叩きゲーム」のような状況になりがちです。

R Markdownを使ってWebページ（htmlファイル）としてレポートを作成すると、これらの作業が簡単になります。この形式では、レポートを長いストリームとして作成できるため、通常は1ページに収まらない大きな図も完全なサイズで保持でき、読み手は単にスクロールするだけで内容を確認できます。また、R Markdown文書の形式はシンプルで、簡単に変更できるため、レポート作成ではなく解析に時間を費やすことができます。

## 文芸的プログラミング（Literate Programming）

理想的には、このような解析レポートは*再現性のある*文書です。エラーが見つかったり、データに新しい被験者が追加されたりした場合でも、レポートを再コンパイルするだけで、新しい結果や修正された結果を得ることができます。図を再作成し、Word文書に貼り付け、詳細な結果を手作業で編集する必要はありません。

ここで重要なRパッケージは[`knitr`](https://yihui.name/knitr/)です。このパッケージを使うと、テキストとコードチャンクを混在させた文書を作成できます。`knitr`が文書を処理すると、コードチャンクが実行され、グラフやその他の結果が最終文書に挿入されます。

このようなアイデアは「文芸的プログラミング」と呼ばれています。

`knitr`では、ほぼすべての種類のテキストとさまざまなプログラミング言語のコードを混在させることができますが、`R Markdown`を使用することをお勧めします。`R Markdown`はMarkdownとRを組み合わせたものです。[Markdown](https://www.markdownguide.org/)は、Webページを作成するための軽量マークアップ言語です。

## R Markdownファイルの作成

RStudio内で、[File] → [New File] → [R Markdown] をクリックすると、次のようなダイアログボックスが表示されます：

![](fig/New_R_Markdown.png){alt='RStudioでの新しいR Markdownファイルダイアログボックスのスクリーンショット'}

デフォルト設定（HTML出力）をそのまま使用しても構いませんが、タイトルを入力してください。

## R Markdownの基本構成

冒頭のテキストチャンク（ヘッダー）は、どの種類の文書を作成するか、選択されたオプションとともにRに指示を与えます。このヘッダーを使用して、文書にタイトル、著者、日付を指定し、作成したい出力形式を指定できます。この例では、HTML文書を作成します。

```
---
title: "Initial R Markdown document"
author: "Karl Broman"
date: "April 23, 2015"
output: html_document
---
```

これらのフィールドは、不要であれば削除できます。ダブルクォートは必須ではありません。必要になるのは、タイトルにコロンを含めたい場合などです。

RStudioは、始めるためのサンプルテキスト付きで文書を作成します。以下のようなチャンクに注意してください：

<pre>
&#96;&#96;&#96;{r}
summary(cars)
&#96;&#96;&#96;
</pre>

これらはRコードチャンクで、`knitr`によって実行され、その結果で置き換えられます。詳細は後ほど説明します。

## Markdown

Markdownは、メールを書くようにテキストをマークアップすることでWebページを作成するシステムです。マークアップされたテキストは*HTMLコード*に変換され、適切なHTMLコードに置き換えられます。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 1

新しいR Markdown文書を作成してください。すべてのRコードチャンクを削除し、Markdownの一部（セクション、斜体のテキスト、箇条書きリスト）を記述します。

文書をWebページに変換してください。

:::::::::::::::  solution

## チャレンジ 1 の解答

RStudioで、[File] → [New File] → [R Markdown...] を選択します。

プレースホルダーテキストを削除し、以下を追加します：

```
# Introduction

## Background on Data

このレポートでは、*gapminder* データセットを使用しています。このデータセットには次の列が含まれます：

* country
* continent
* year
* lifeExp
* pop
* gdpPercap

## Background on Methods

```

その後、ツールバーの「Knit」ボタンをクリックして、HTMLドキュメント（Webページ）を生成します。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## もう少しMarkdownについて

ハイパーリンクを作成するには、以下のように記述します：
`[Carpentries ホームページ](https://carpentries.org/)`。

画像ファイルを挿入するには、以下のように記述します：
`![The Carpentries Logo](https://carpentries.org/assets/img/TheCarpentries.svg)`

添字（例：F~2~）は `F~2~`、上付き文字（例：F^2^）は `F^2^` と記述します。

[LaTeX](https://www.latex-project.org/)で方程式を書く方法を知っていれば、`$ $` や `$$ $$` を使って数式を挿入できます。
例えば、`$E = mc^2$` または次のような形で記述できます：

```
$$y = \mu + \sum_{i=1}^p \beta_i x_i + \epsilon$$
```

Markdownの構文は、RStudioのツールバーの「Help」フィールドにある
"Markdown Quick Reference" で確認できます。

## Rコードチャンク

Markdownの真の力は、マークダウンにコードチャンクを組み合わせるところにあります。これがR Markdownです。処理されると、Rコードは実行され、生成される図などが最終文書に挿入されます。

基本的なコードチャンクは以下のようになります：

<pre>
&#96;&#96;&#96;{r load_data}
gapminder <- read.csv("gapminder.csv")
&#96;&#96;&#96;
</pre>

つまり、<code>\`\`\`{r chunk\_name}</code> と <code>\`\`\`</code> の間にRコードを記述します。各チャンクには一意の名前を付けるべきです。これによりエラーの修正が容易になり、図が生成される場合、生成されるファイル名はそのコードチャンクの名前に基づきます。RStudioでは、ショートカット <kbd>Ctrl</kbd>\+<kbd>Alt</kbd>\+<kbd>I</kbd>（WindowsおよびLinux）、または <kbd>Cmd</kbd>\+<kbd>Option</kbd>\+<kbd>I</kbd>（Mac）を使用してコードチャンクを素早く作成できます。

:::::::::::::::::::::::::::::::::::::::  チャレンジ

## チャレンジ 2

以下のコードチャンクを追加してください：

- ggplot2パッケージをロードする
- gapminderデータを読み込む
- プロットを作成する

:::::::::::::::  解答

## チャレンジ 2 の解答

<pre>
&#96;&#96;&#96;{r load-ggplot2}
library("ggplot2")
&#96;&#96;&#96;
</pre>

<pre>
&#96;&#96;&#96;{r read-gapminder-data}
gapminder <- read.csv("gapminder.csv")
&#96;&#96;&#96;
</pre>

<pre>
&#96;&#96;&#96;{r make-plot}
plot(lifeExp ~ year, data = gapminder)
&#96;&#96;&#96;
</pre>

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## コンパイルの仕組み

「Knit」ボタンを押すと、R Markdown文書は[`knitr`](https://yihui.name/knitr)によって処理され、プレーンなMarkdown文書（および必要に応じて一連の図ファイル）が生成されます。Rコードは実行され、その入力と出力に置き換えられます。図が生成される場合、それらのリンクが文書に含まれます。

その後、Markdown文書と図ファイルは[`pandoc`](https://pandoc.org/)ツールによって処理され、図が埋め込まれたHTMLファイルに変換されます。

<img src="fig/14-knitr-markdown-rendered-rmd_to_html_fig-1.png" style="display: block; margin: auto auto auto 0;" />

## チャンクオプション

コードチャンクの処理方法を変更するためのオプションがさまざま用意されています。例：

- `echo=FALSE`を使用してコード自体を非表示にする。
- `results="hide"`を使用して結果の印刷を抑制する。
- `eval=FALSE`を使用してコードを表示するが評価しない。
- `warning=FALSE`や`message=FALSE`を使用して警告やメッセージを非表示にする。
- `fig.height`や`fig.width`を使用して生成される図のサイズ（インチ単位）を制御する。

たとえば、以下のように記述します：

<pre>
&#96;&#96;&#96;{r load_libraries, echo=FALSE, message=FALSE}
library("dplyr")
library("ggplot2")
&#96;&#96;&#96;
</pre>

:::::::::::::::::::::::::::::::::::::::  チャレンジ

## チャレンジ 3

チャンクオプションを使用して図のサイズを制御し、コードを非表示にしてください。

:::::::::::::::  解答

## チャレンジ 3 の解答

<pre>
&#96;&#96;&#96;{r echo = FALSE, fig.width = 3}
plot(faithful)
&#96;&#96;&#96;
</pre>

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

R Markdownのすべてのチャンクオプションは、RStudioのツールバーの「Help」フィールド内「Cheatsheets」セクションにある「R Markdown Cheat Sheet」で確認できます。

## インラインRコード

レポート内の*すべての*数値を再現可能にすることができます。  
<code>\`r</code> と <code>\`</code> を使用してインラインコードチャンクを作成します。  
例: ``` `r round(some_value, 2)` ```. このコードは実行され、結果の*値*に置き換えられます。

インラインチャンクが行をまたいで分割されないように注意してください。

計算や変数の定義を行う大きなコードチャンクを、`include=FALSE` を指定して記述するのが良いでしょう  
（これは `echo=FALSE` と `results="hide"` を指定するのと同じです）。

丸め処理は、場合によって出力に差異を生じさせることがあります。たとえば `2.0` を期待していても、`round(2.03, 1)` は `2` だけを返します。

[R/broman](https://github.com/kbroman/broman) パッケージの  
[`myround`](https://github.com/kbroman/broman/blob/master/R/myround.R) 関数は、この問題を処理します。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 4

インラインRコードを少し試してみましょう。

:::::::::::::::  solution

## チャレンジ 4 の解答

以下は、2 + 2 = ``4`` を求めるインラインコードの例です。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 他の出力オプション

R Markdown を PDF または Word ドキュメントに変換することもできます。  
"ニット" ボタン横の小さな三角形をクリックすると、ドロップダウンメニューが表示されます。  
または、ファイルの冒頭ヘッダーに `pdf_document` または `word_document` を指定することも可能です。

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント: PDFドキュメントの作成

PDF ドキュメント（.pdf）を作成するには、追加のソフトウェアが必要になる場合があります。  
Rパッケージ `tinytex` は、このプロセスを簡単にするツールを提供します。  
`tinytex` をインストールした後、`tinytex::install_tinytex()` を実行して必要なソフトウェアをインストールしてください（この作業は一度だけでOKです）。  
その後、PDFにニットする際に `tinytex` が自動的に必要な LaTeX パッケージを検出し、インストールします。  
詳細については、[tinytexの公式ウェブサイト](https://yihui.org/tinytex/) をご覧ください。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント: RStudio のビジュアルマークダウン編集

RStudio バージョン 1.4 以降には、ビジュアルマークダウン編集モードが含まれています。  
このモードでは、マークダウン表記（例: `**太字**`）が入力中にフォーマットされた外観（**太字**）に変換されます。  
また、このモードには上部に基本的なフォーマットボタンを含むツールバーがあり、一般的なワープロソフトに似た操作が可能です。  
ビジュアル編集モードは、R Markdown ドキュメントの右上隅にある  
![](fig/visual_mode_icon.png){alt='RStudio のビジュアル編集モードのオンオフ切り替えボタンのアイコン（コンパスのような形）'} ボタンを押すことでオン/オフを切り替えられます。

::::::::::::::::::::::::::::::::::::::::::::::::::

## リソース

- [Knitr in a knutshell tutorial](https://kbroman.org/knitr_knutshell)
- [Dynamic Documents with R and knitr](https://www.amazon.com/exec/obidos/ASIN/1482203537/7210-20) (書籍)
- [R Markdown documentation](https://rmarkdown.rstudio.com)
- [R Markdown cheat sheet](https://www.rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf)
- [Getting started with R Markdown](https://www.rstudio.com/resources/webinars/getting-started-with-r-markdown/)
- [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/) (Rstudio チームの書籍)
- [Reproducible Reporting](https://www.rstudio.com/resources/webinars/reproducible-reporting/)
- [The Ecosystem of R Markdown](https://www.rstudio.com/resources/webinars/the-ecosystem-of-r-markdown/)
- [Introducing Bookdown](https://www.rstudio.com/resources/webinars/introducing-bookdown/)

:::::::::::::::::::::::::::::::::::::::: keypoints

- R Markdown で作成したレポートに、R で書かれたソフトウェアを組み合わせる。
- チャンクオプションを指定してフォーマットを制御する。
- `knitr` を使用して、これらのドキュメントを PDF や他の形式に変換する。

::::::::::::::::::::::::::::::::::::::::::::::::::


