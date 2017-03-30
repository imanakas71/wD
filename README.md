# cD
同一テキスト内で、似たような行を探して表示するプログラム

## 概要
python 2.7 で組んだプログラムです。  　
漫画の自炊などをやっていると気まぐれで「！」を全角や半角にしてしまったり、タイトルの句読点を入れ損なったりして後でいろいろ後悔することがあると思います。   
500冊、1000冊と数が増えていくと、タイトルの表記ゆれの数も気になりだしますがかといってもう何がどうオカシイのかわかりません。   
このpython スクリプトはそんな時、ちょっとだけ役に立ちます。   

たとえば電子書籍の書斎一覧として、以下のようなテキストデータがあったとします。  

```
斉藤くんは、召喚魔法で異世界に飛ばされたようです。 第04巻
斉藤くんは召喚魔法で異世界に飛ばされたようです 第14巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第01巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第02巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第03巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第05巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第06巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第07巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第08巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第10巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第11巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第12巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第13巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第15巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第16巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第17巻
斉藤くんは召喚魔法で異世界に飛ばされたようです。 第18巻
斉藤君は召喚魔法で異世界に飛ばされたようです。 第09巻

```

こういったテキストに対して、「第xx巻」の部分は無視して、右から一つ目のスペースを起点に本の名前を左右に分けてしまって、左だけの類似点を評価するというプログラムです。   
```
類似の可能性: 0.979   :斉藤くんは召喚魔法で異世界に飛ばされたようです。(15)<-->斉藤くんは、召喚魔法で異世界に飛ばされたようです。(1)
類似の可能性: 0.978   :斉藤くんは召喚魔法で異世界に飛ばされたようです。(15)<-->斉藤くんは召喚魔法で異世界に飛ばされたようです(1)
類似の可能性: 0.936   :斉藤くんは召喚魔法で異世界に飛ばされたようです。(15)<-->斉藤君は召喚魔法で異世界に飛ばされたようです。(1)

```

*多数派は（）内の数字が多く、少数派は少なくなっています。また、必ず左側の数字が大きくなっているケースを抽出しています。
*タイトルが短くなると一文字あたりの重要度が増えてしまい、一文字違うだけで「類似度」が大きく下がってしまいますので、短いタイトルのものは検出にひっかかりにくいです。
*現在は類似度85%以上で検出しているので、「鈴木君」は残念なことに検出されませんでした。

## 使い方
自炊した書籍を保存しているフォルダー内で、まず本の一覧を作ります。Windowsだと   

```
% tree /f >booksWin.txt
```

cygwin, Linuxなどで

```
% cDpros.sh booksWin.txt
```
で。  books.txt は 生成できます
あと、シェルの環境変数`PYTHOIOENCODING`を`utf-8`にしないとリダイレクトができません。  。 


```
PYTHONIOENCODING=utf-8
export PYTHONIOENCODING
```

環境変数、books.txt ともに出来上がったら   


```
% cD.py
```

だけで、類似の可能性があるファイルを提示してくれます。

### 愚痴
それにしてもアレですね。出版業界の人たちって「君」とか「様」とか、どうしてひらがなにして開いちゃったり、本のタイトルなのに読点まで付けたがったりするんでしょうね。あれは工夫でもなんでもないと思うんでうすよ。
