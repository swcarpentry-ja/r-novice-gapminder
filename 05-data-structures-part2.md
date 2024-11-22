---
title: データフレームの操作
teaching: 20
exercises: 10
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- 行や列を追加または削除する。
- 2 つのデータフレームを結合する。
- データフレームのサイズ、列のクラス、名前、最初の数行などの基本的なプロパティを表示する。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- データフレームをどのように操作できますか？

::::::::::::::::::::::::::::::::::::::::::::::::::



これまでに、R の基本的なデータ型とデータ構造について学びました。以降の作業は、それらのツールを操作することに集約されます。最も頻繁に登場するのは、CSV ファイルから情報を読み込んで作成するデータフレームです。このレッスンでは、データフレームの操作についてさらに学びます。

## データフレームに列や行を追加する

データフレームの列はベクトルであるため、列全体でデータ型が一貫しています。そのため、新しい列を追加したい場合は、まず新しいベクトルを作成します：




``` r
age <- c(2, 3, 5)
cats
```

``` output
    coat weight likes_catnip
1 calico    2.1            1
2  black    5.0            0
3  tabby    3.2            1
```

これを列として追加するには、次のようにします：


``` r
cbind(cats, age)
```

``` output
    coat weight likes_catnip age
1 calico    2.1            1   2
2  black    5.0            0   3
3  tabby    3.2            1   5
```

ただし、データフレームの行数と異なる要素数を持つベクトルを追加しようとすると失敗します：


``` r
age <- c(2, 3, 5, 12)
cbind(cats, age)
```

``` error
Error in data.frame(..., check.names = FALSE): arguments imply differing number of rows: 3, 4
```

``` r
age <- c(2, 3)
cbind(cats, age)
```

``` error
Error in data.frame(..., check.names = FALSE): arguments imply differing number of rows: 3, 2
```

なぜ失敗するのでしょうか？R は、新しい列の各行に 1 つの要素が必要だと考えています：


``` r
nrow(cats)
```

``` output
[1] 3
```

``` r
length(age)
```

``` output
[1] 2
```

したがって、`nrow(cats)` と `length(age)` が等しい必要があります。新しいデータフレームを作成して、`cats` に上書きしてみましょう。


``` r
age <- c(2, 3, 5)
cats <- cbind(cats, age)
```

次に、行を追加してみましょう。データフレームの行はリストであることを既に学びました：


``` r
newRow <- list("tortoiseshell", 3.3, TRUE, 9)
cats <- rbind(cats, newRow)
```

新しい行が正しく追加されたことを確認します。


``` r
cats
```

``` output
           coat weight likes_catnip age
1        calico    2.1            1   2
2         black    5.0            0   3
3         tabby    3.2            1   5
4 tortoiseshell    3.3            1   9
```

## 行を削除する

データフレームに行や列を追加する方法を学びました。次に、行を削除する方法を見てみましょう。


``` r
cats
```

``` output
           coat weight likes_catnip age
1        calico    2.1            1   2
2         black    5.0            0   3
3         tabby    3.2            1   5
4 tortoiseshell    3.3            1   9
```

最後の行を削除したデータフレームを取得するには：


``` r
cats[-4, ]
```

``` output
    coat weight likes_catnip age
1 calico    2.1            1   2
2  black    5.0            0   3
3  tabby    3.2            1   5
```

コンマの後に何も指定しないことで、4 行目全体を削除することを示します。

複数の行を削除することもできます。たとえば、次のようにベクトル内に行番号を指定します：`cats[c(-3,-4), ]`

## 列を削除する

データフレームの列を削除することもできます。「age」列を削除する場合、変数番号またはインデックスを使用する方法があります。


``` r
cats[,-4]
```

``` output
           coat weight likes_catnip
1        calico    2.1            1
2         black    5.0            0
3         tabby    3.2            1
4 tortoiseshell    3.3            1
```

コンマの前に何も指定しないことで、すべての行を保持することを示します。

または、インデックス名と `%in%` 演算子を使用して列を削除することもできます。`%in%` 演算子は、左側の引数（ここでは `cats` の名前）の各要素について「この要素は右側の引数に含まれますか？」と尋ねます。


``` r
drop <- names(cats) %in% c("age")
cats[,!drop]
```

``` output
           coat weight likes_catnip
1        calico    2.1            1
2         black    5.0            0
3         tabby    3.2            1
4 tortoiseshell    3.3            1
```

論理演算子（`%in%` など）による部分集合化については、次のエピソードで詳しく説明します。詳細は [論理演算を使用した部分集合化](06-data-subsetting.Rmd) を参照してください。

## データフレームの結合

データフレームにデータを追加する際に覚えておくべき重要な点は、*列はベクトル、行はリスト*であることです。2 つのデータフレームを `rbind` を使用して結合することもできます：


``` r
cats <- rbind(cats, cats)
cats
```

``` output
           coat weight likes_catnip age
1        calico    2.1            1   2
2         black    5.0            0   3
3         tabby    3.2            1   5
4 tortoiseshell    3.3            1   9
5        calico    2.1            1   2
6         black    5.0            0   3
7         tabby    3.2            1   5
8 tortoiseshell    3.3            1   9
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 1

次の構文を使用して、新しいデータフレームを R 内で作成できます：


``` r
df <- data.frame(id = c("a", "b", "c"),
                 x = 1:3,
                 y = c(TRUE, TRUE, FALSE))
```

以下の情報を持つデータフレームを作成してください：

- 名
- 姓
- ラッキーナンバー

次に、`rbind` を使用して隣の人のエントリを追加します。最後に、`cbind` を使用して「コーヒーブレイクの時間ですか？」という質問への各人の回答を含む列を追加してください。

:::::::::::::::  solution

## チャレンジ 1 の解答


``` r
df <- data.frame(first = c("Grace"),
                 last = c("Hopper"),
                 lucky_number = c(0))
df <- rbind(df, list("Marie", "Curie", 238) )
df <- cbind(df, coffeetime = c(TRUE, TRUE))
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 実用的な例

これまで、猫データを使ってデータフレーム操作の基本を学びました。次に、これらのスキルを使用して、より現実的なデータセットを扱います。以前ダウンロードした `gapminder` データセットを読み込んでみましょう：


``` r
gapminder <- read.csv("data/gapminder_data.csv")
```

:::::::::::::::::::::::::::::::::::::::::  callout

## その他のヒント

- タブ区切り値ファイル（.tsv）を扱う場合は、区切り文字として `"\\t"` を指定するか、`read.delim()` を使用します。

- ファイルをインターネットから直接ダウンロードしてコンピュータの指定したローカルフォルダに保存するには、`download.file` 関数を使用できます。保存されたファイルを `read.csv` 関数で読み込む例：


``` r
download.file("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/main/episodes/data/gapminder_data.csv", destfile = "data/gapminder_data.csv")
gapminder <- read.csv("data/gapminder_data.csv")
```

- また、ファイルパスの代わりに Web アドレスを `read.csv` に指定して、ファイルを直接 R に読み込むこともできます。この場合、ローカルにファイルを保存する必要はありません。例：


``` r
gapminder <- read.csv("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/main/episodes/data/gapminder_data.csv")
```

- [readxl パッケージ](https://cran.r-project.org/package=readxl) を使用すると、Excel スプレッドシートをプレーンテキストに変換せずに直接読み込むことができます。

- `"stringsAsFactors"` 引数を使用すると、文字列を因子として読み込むか文字列として読み込むかを指定できます。R バージョン 4.0 以降では、デフォルトで文字列は文字型として読み込まれますが、古いバージョンでは因子として読み込まれるのがデフォルトでした。詳細は[前のエピソード](https://swcarpentry.github.io/r-novice-gapminder/04-data-structures-part1.html#check-your-data-for-factors)のコールアウトを参照してください。

::::::::::::::::::::::::::::::::::::::::::::::::::

`gapminder` データセットを調べてみましょう。最初に行うべきことは、`str` を使用してデータの構造を確認することです：


``` r
str(gapminder)
```

``` output
'data.frame':	1704 obs. of  6 variables:
 $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
 $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
 $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
 $ continent: chr  "Asia" "Asia" "Asia" "Asia" ...
 $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
 $ gdpPercap: num  779 821 853 836 740 ...
```

`gapminder` の構造を調べる別の方法として、`summary` 関数を使用します。この関数は R のさまざまなオブジェクトで使用できます。データフレームの場合、`summary` は各列の数値的、表形式、または記述的な概要を提供します。数値または整数型の列は記述統計（四分位数や平均値）で、文字列型の列はその長さ、クラス、モードで説明されます。


``` r
summary(gapminder)
```

``` output
   country               year           pop             continent        
 Length:1704        Min.   :1952   Min.   :6.001e+04   Length:1704       
 Class :character   1st Qu.:1966   1st Qu.:2.794e+06   Class :character  
 Mode  :character   Median :1980   Median :7.024e+06   Mode  :character  
                    Mean   :1980   Mean   :2.960e+07                     
                    3rd Qu.:1993   3rd Qu.:1.959e+07                     
                    Max.   :2007   Max.   :1.319e+09                     
    lifeExp        gdpPercap       
 Min.   :23.60   Min.   :   241.2  
 1st Qu.:48.20   1st Qu.:  1202.1  
 Median :60.71   Median :  3531.8  
 Mean   :59.47   Mean   :  7215.3  
 3rd Qu.:70.85   3rd Qu.:  9325.5  
 Max.   :82.60   Max.   :113523.1  
```

`str` や `summary` 関数と合わせて、`typeof` 関数を使ってデータフレームの個々の列を調べることもできます：


``` r
typeof(gapminder$year)
```

``` output
[1] "integer"
```

``` r
typeof(gapminder$country)
```

``` output
[1] "character"
```

``` r
str(gapminder$country)
```

``` output
 chr [1:1704] "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
```

データフレームの次元に関する情報も調べることができます。  
`str(gapminder)` の出力によると、`gapminder` には 6 つの変数の 1704 個の観測値があります。このことを覚えた上で、次のコードが何を返すか考えてみてください：


``` r
length(gapminder)
```

``` output
[1] 6
```

データフレームの長さが行数（1704）であると考えるのが妥当ですが、実際にはそうではありません。データフレームは*ベクトルや因子のリスト*で構成されていることを思い出してください：


``` r
typeof(gapminder)
```

``` output
[1] "list"
```

`length` が 6 を返した理由は、`gapminder` が 6 列のリストで構築されているためです。データセットの行数と列数を取得するには次のようにします：


``` r
nrow(gapminder)
```

``` output
[1] 1704
```

``` r
ncol(gapminder)
```

``` output
[1] 6
```

あるいは、両方を一度に取得するには：


``` r
dim(gapminder)
```

``` output
[1] 1704    6
```

すべての列のタイトルを調べることもできます。後でアクセスする際に便利です：


``` r
colnames(gapminder)
```

``` output
[1] "country"   "year"      "pop"       "continent" "lifeExp"   "gdpPercap"
```

ここで、R が報告する構造が自分の直感や予想と一致しているかどうかを確認することが重要です。各列のデータ型が妥当かどうかを確認してください。そうでない場合は、これまで学んだ R のデータ解釈の仕組みや、一貫性の重要性に基づいて問題を解決する必要があります。

データ型や構造が合理的であることを確認したら、データの探索を開始しましょう。最初の数行を確認します：


``` r
head(gapminder)
```

``` output
      country year      pop continent lifeExp gdpPercap
1 Afghanistan 1952  8425333      Asia  28.801  779.4453
2 Afghanistan 1957  9240934      Asia  30.332  820.8530
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
4 Afghanistan 1967 11537966      Asia  34.020  836.1971
5 Afghanistan 1972 13079460      Asia  36.088  739.9811
6 Afghanistan 1977 14880372      Asia  38.438  786.1134
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 2

データの最後の数行や中間のいくつかの行も確認するのが良い習慣です。これをどのように行いますか？

特に中間の行を探すのは難しくありませんが、ランダムな行をいくつか取得することもできます。これをどのようにコード化しますか？

:::::::::::::::  solution

## チャレンジ 2 の解答

最後の数行を確認するには、R に既にある関数を使用すれば簡単です：

```r
tail(gapminder)
tail(gapminder, n = 15)
```

では、途中の任意の行を確認するにはどうすればよいでしょうか？

## ヒント: いくつかの方法で達成できます

ここではネストされた関数（関数を別の関数の引数として渡す）を使用する一例を示します。この考え方は新しいように思えるかもしれませんが、既に使用しています。  
例えば、`my_dataframe[rows, cols]` は指定された行と列のデータフレームを表示します。データフレームの最後の行を取得するにはどうしますか？R にはそのための関数があります。また、（擬似ランダムな）サンプルを取得するにはどうすればよいでしょうか？

```r
gapminder[sample(nrow(gapminder), 5), ]
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

再現性のある分析を確保するために、コードをスクリプトファイルに保存し、後で再利用できるようにしましょう。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 3

File -> New File -> R Script に移動し、`gapminder` データセットを読み込むための R スクリプトを作成します。このスクリプトを `scripts/` ディレクトリに保存し、バージョン管理に追加してください。

その後、`source` 関数を使用してスクリプトを実行します。ファイルパスを引数として指定するか、RStudio の「Source」ボタンを押します。

:::::::::::::::  solution

## チャレンジ 3 の解答

`source` 関数はスクリプト内で別のスクリプトを使用するために使用できます。同じ種類のファイルを何度も読み込む必要がある場合、一度スクリプトとして保存すれば、以降はそれを繰り返し利用できます。


``` r
download.file("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/main/episodes/data/gapminder_data.csv", destfile = "data/gapminder_data.csv")
gapminder <- read.csv(file = "data/gapminder_data.csv")
```

データを `gapminder` 変数に読み込むには次のようにします：


``` r
source(file = "scripts/load-gapminder.R")
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 4

`str(gapminder)` の出力をもう一度読み、リストやベクトルについて学んだこと、および `colnames` や `dim` の出力を活用して、`str` が表示する内容を説明してください。理解できない部分があれば、隣の人と相談してみてください。

:::::::::::::::  solution

## チャレンジ 4 の解答

オブジェクト `gapminder` はデータフレームで、列は次のようになっています：

-

 `country` と `continent` は文字列（character）。
- `year` は整数型のベクトル。
- `pop`、`lifeExp`、`gdpPercap` は数値型のベクトル。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- 新しい列をデータフレームに追加するには `cbind()` を使用します。
- 新しい行をデータフレームに追加するには `rbind()` を使用します。
- データフレームから行を削除します。
- データフレームの構造を理解するために、`str()`、`summary()`、`nrow()`、`ncol()`、`dim()`、`colnames()`、`head()`、`typeof()` を使用します。
- `read.csv()` を使用して CSV ファイルを読み込みます。
- データフレームの `length()` が何を表しているのか理解します。

::::::::::::::::::::::::::::::::::::::::::::::::::
