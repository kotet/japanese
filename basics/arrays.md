# 配列

Dの配列には**静的**と**動的**の二種類があります。
配列へのあらゆるアクセスは境界チェックされています
(コンパイラが境界チェックが不要だと証明できる場合を除く）。
境界チェックに失敗すると、アプリケーションを終了させる`RangeError`を発生させます。
勇敢な人は、安全性を犠牲にして速度を向上させるため、
このセーフティ機能を`-boundschecks=off`コンパイラフラグで無効化できます。

#### 静的配列

静的配列は関数の中で定義された場合スタックに、そうでなければ静的メモリに保持されます。
その長さは固定されており、コンパイル時に判明しています。静的配列の型は固定のサイズを含みます:

    int[8] arr;

`arr`の型は`int[8]`です。配列のサイズは型の次に表記され、
C/C++のように変数名の後には来ないことに注意してください。

#### 動的配列

動的配列はヒープに保持され、実行時に伸び縮みできます。
動的配列は`new`式と長さを使い作成します:

    int size = 8; // 実行時変数
    int[] arr = new int[size];

`arr`の型は`int[]`で、これは**スライス**といいます。
スライスは連続したメモリブロックに対するビューであり、
[次のセクション](basics/slices)でより詳細に説明されます。
多次元配列は`auto arr = new int[3][3]`構文で簡単に作成できます。

#### 配列操作とプロパティ

配列は`~`演算子で結合でき、このとき新しい動的配列が作られます。

数学的演算がたとえば`c[] = a[] + b[]`のような構文を使って配列全体に適用できます。
これは`a`と`b`のすべての要素を`c[0] = a[0] + b[0]`、 `c[1] = a[1] + b[1]`
などというように足し合わせます。配列全体と単一の値でも操作を行えます:

    a[] *= 2; // すべての要素に2をかける
    a[] %= 26; // aの全体にモジュロ26を計算する

これらの操作はそれを一度に行える特殊なプロセッサ命令を使うよう
コンパイラが最適化するかもしれません。

動的配列、静的配列の両方に`.length`プロパティがあり、これは静的配列では読み込み専用ですが、
動的配列の場合そのサイズを動的に変更するのに使えます。`.dup`プロパティは配列のコピーを作ります。

`arr[idx]`構文で配列をインデクシングするとき、特殊な`$`記号は配列の長さを表します。
例えば、`arr[$ - 1]`は最後の要素を参照し、`arr[arr.length - 1]`を短縮したものです。

### エクササイズ

`encrypt`関数を完成させて秘密のメッセージを暗号化しましょう。
テキストは**シーザー暗号**を使って暗号化されなければなりません。
これは特定のインデックスを用いてアルファベットの文字をシフトするものです。
暗号化されるテキストには`a-z`の範囲の文字しか含まれていないため、
問題が簡単になるはずです。

解法は[こちら](https://github.com/dlang-tour/core/issues/227)で閲覧できます。

### 掘り下げる

- [Arrays in _Programming in D_](http://ddili.org/ders/d.en/arrays.html)
- [D Slices](https://dlang.org/d-array-article.html)
- [Array specification](https://dlang.org/spec/arrays.html)

## {SourceCode:incomplete}

```d
import std.stdio : writeln;

/**
配列`input`内の各文字を
`shift`文字シフトします。
文字の範囲は`a-z`に制限されており、
zの次の文字はaです。

Params:
    input = シフトする配列
    shift = 各文字をシフトする量
Returns:
    シフトされた文字の配列
*/
char[] encrypt(char[] input, char shift)
{
    auto result = input.dup;
    // TODO: 各文字をシフトする
    return result;
}

void main()
{
    // シーザー暗号でメッセージを暗号化し、
    // 係数16でシフトします!
    char[] toBeEncrypted = [ 'w','e','l','c',
      'o','m','e','t','o','d',
      // 最後の,はあってもよく、単に無視されます!
    ];
    writeln("Before: ", toBeEncrypted);
    auto encrypted = encrypt(toBeEncrypted, 16);
    writeln("After: ", encrypted);

    // アルゴリズムが期待通りに動くか確認します
    assert(encrypted == [ 'm','u','b','s','e',
            'c','u','j','e','t' ]);
}
```
