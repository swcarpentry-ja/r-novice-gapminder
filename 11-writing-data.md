---
title: データの書き出し
teaching: 10
exercises: 10
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Rからプロットやデータを保存できるようになる。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Rで作成したプロットやデータをどのように保存できますか？

::::::::::::::::::::::::::::::::::::::::::::::::::



## プロットの保存

すでに `ggplot2` で作成した最新のプロットを `ggsave` コマンドを使って保存する方法を学びました。
復習として：


``` r
ggsave("My_most_recent_plot.pdf")
```

RStudio の「Plot」ウィンドウで「Export」ボタンを使用してプロットを保存することもできます。
これにより、.pdf や .png、.jpg などの形式で保存するオプションが表示されます。

プロットを事前に「Plot」ウィンドウに作成せずに保存したい場合もあります。
たとえば、複数ページのPDFドキュメントを作成し、それぞれ異なるプロットを表示したい場合や、
ループでファイルの複数のサブセットを処理して、各サブセットからデータをプロットし、それぞれのプロットを保存したい場合があります。

この場合、より柔軟なアプローチを使用できます。
`pdf` 関数を使用して新しいPDFデバイスを作成できます。この関数の引数を使用してサイズや解像度を制御できます。


``` r
pdf("Life_Exp_vs_time.pdf", width=12, height=4)
ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=country)) +
  geom_line() +
  theme(legend.position = "none")

# PDFデバイスをオフにすることを忘れないでください！
dev.off()
```

このドキュメントを開いて確認してみてください。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 1

'pdf' コマンドを変更して、PDFの2ページ目を作成し、
同じデータを使って `facet_grid` を使用して大陸ごとに1つのパネルを表示するプロットを追加してください。

:::::::::::::::  solution

## チャレンジ 1 の解答


``` r
pdf("Life_Exp_vs_time.pdf", width = 12, height = 4)
p <- ggplot(data = gapminder, aes(x = year, y = lifeExp, colour = country)) +
  geom_line() +
  theme(legend.position = "none")
p
p + facet_grid(~continent)
dev.off()
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

`jpeg` や `png` などのコマンドも同様に使用して、異なる形式のドキュメントを作成できます。

## データの書き出し

Rからデータを書き出したい場合があります。

`write.table` 関数を使用してこれを行うことができます。
この関数は以前学んだ `read.table` に非常に似ています。

データクリーニングスクリプトを作成しましょう。
この分析では、オーストラリアの gapminder データに焦点を当てます：


``` r
aust_subset <- gapminder[gapminder$country == "Australia",]

write.table(aust_subset,
  file="cleaned-data/gapminder-aus.csv",
  sep=","
)
```

シェルに戻り、データが正しく表示されるか確認しましょう：


``` bash
head cleaned-data/gapminder-aus.csv
```

``` output
"country","year","pop","continent","lifeExp","gdpPercap"
"61","Australia",1952,8691212,"Oceania",69.12,10039.59564
"62","Australia",1957,9712569,"Oceania",70.33,10949.64959
"63","Australia",1962,10794968,"Oceania",70.93,12217.22686
"64","Australia",1967,11872264,"Oceania",71.1,14526.12465
"65","Australia",1972,13177000,"Oceania",71.93,16788.62948
"66","Australia",1977,14074100,"Oceania",73.49,18334.19751
"67","Australia",1982,15184200,"Oceania",74.74,19477.00928
"68","Australia",1987,16257249,"Oceania",76.32,21888.88903
"69","Australia",1992,17481977,"Oceania",77.56,23424.76683
```

あれ、ちょっと想定と違いますね。なぜか引用符がついています。
また、行番号が意味を持たないようです。

ヘルプファイルを見て、この挙動を変更する方法を確認してみましょう。


``` r
?write.table
```

デフォルトでは、Rは文字ベクトルをファイルに書き出す際に引用符で囲みます。
また、行と列の名前も書き出します。

これを修正しましょう：


``` r
write.table(
  gapminder[gapminder$country == "Australia",],
  file="cleaned-data/gapminder-aus.csv",
  sep=",", quote=FALSE, row.names=FALSE
)
```

もう一度シェルスキルを使ってデータを確認しましょう：


``` bash
head cleaned-data/gapminder-aus.csv
```

``` output
country,year,pop,continent,lifeExp,gdpPercap
Australia,1952,8691212,Oceania,69.12,10039.59564
Australia,1957,9712569,Oceania,70.33,10949.64959
Australia,1962,10794968,Oceania,70.93,12217.22686
Australia,1967,11872264,Oceania,71.1,14526.12465
Australia,1972,13177000,Oceania,71.93,16788.62948
Australia,1977,14074100,Oceania,73.49,18334.19751
Australia,1982,15184200,Oceania,74.74,19477.00928
Australia,1987,16257249,Oceania,76.32,21888.88903
Australia,1992,17481977,Oceania,77.56,23424.76683
```

これでよさそうです！

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 2

gapminder データを1990年以降に収集されたデータポイントのみを含むようにサブセット化する
データクリーニングスクリプトファイルを作成してください。

このスクリプトを使用して、新しいサブセットを `cleaned-data/` ディレクトリに保存してください。

:::::::::::::::  solution

## チャレンジ 2 の解答


``` r
write.table(
  gapminder[gapminder$year > 1990, ],
  file = "cleaned-data/gapminder-after1990.csv",
  sep = ",", quote = FALSE, row.names = FALSE
)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- RStudioでプロットを保存するには「Export」ボタンを使用する。
- タブ形式のデータを保存するには `write.table` を使用する。

::::::::::::::::::::::::::::::::::::::::::::::::::


