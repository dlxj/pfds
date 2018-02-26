# 演習問題 8.7

> 実時間両端キューにおいて
>
> - 挿入で 1 
> - 削除で 2
>
> 停止計算を進行させれば、次の回転より前にスケジュールが実行完了するには十分であることを示せ

- 連続で cons, tail する場合を考える
- snoc, init は省略

## cons 

次の回転までの cons の回数と、回転時に作られるスケジュールの長さを比較する。

### 次の回転までの cons の回数

cons によって回転が開始するとき $|f| = c|r| + 2$となっている。

$|r| = m$ とすると、$|f| = cm + 2$ となり、 $i = \lfloor\frac{|r| + |f|}{2}\rfloor = \lfloor\frac{c + 1}{2}\rfloor m+1$ である。

回転が完了するときの先頭ストリームの長さは $i$ であり、次の回転までの cons の回数は、$ci+ 2- i=(c-1)i+2$ 回となる。

### 回転時に作られるスケジュールの長さ

`take i f` より先頭側スケジュールの長さは $i$ である。

末尾側スケジュール長さは `rotateDrop r i f` を完全に消化する回数である。

rotateDrop は 1 ステップ毎に $r$  を 1 回、$f$ を $c$ 回（合計 $i$ まで）進行させる。合計のステップ数を比較すると $|r|+1 - \frac{i}{c} = m+1 - (\lfloor\frac{c + 1}{2c}\rfloor m+1) = \lfloor\frac{c - 1}{2c}\rfloor m > 0 \quad (c>1, m>0)$ なので、rotateDrop による末尾側スケジュールの長さは $|r|+1$ として考える。

- 比較を簡単にするため $|r|+1$ としていることに注意

スケジュールは先頭側も末尾側も同時に進行するが、$i \ge |r|+1 \quad (c>1)$ より、スケジュールが完了するまでにかかるステップ数は $i$ となる。

以上より、$(c-1)i+2-i=(c-2)i+2 > 0 \quad (c > 1)$ であるため、停止計算を 1 つずつ実行すれば次の回転までに間に合う。

## tail

### 次の回転までの tail の回数

tail によって回転が開始するとき $|r| = c|f| + 1$となっている。（consとは関係が逆）

$|f| = m$ とすると、$|r| = cm + 2$ となり、 $i = \lfloor\frac{|r| + |f|}{2}\rfloor = \lfloor\frac{c + 1}{2}\rfloor m+1$ である。また、$j=|f|+|r|-i = \lceil\frac{c + 1}{2}\rceil m+1$

回転が完了するときの先頭ストリームの長さは $j$ であり、次の回転までの tail の回数は、$j - \frac{i-2}{c}$ 回となる。

### 回転時に作られるスケジュールの長さ

`take j f` より先頭側スケジュールの長さは $j$ である。

末尾側スケジュール長さは `rotateDrop f j r` を完全に消化する回数である。

cons の場合と同じように考えると、末尾側スケジュールの長さは $|f|+1$ として考える。

$j \ge |f|+1 \quad (c>1)​$ より、スケジュールが完了するまでにかかるステップ数は $j​$ となる。

以上より、停止計算を 2 つずつ実行することを考慮して、$2(j-\frac{i-2}{c})-j = j - \frac{2(i-2)}{c} \ge i - \frac{2(i-2)}{c} = \frac{(c-2)i+4}{c}>0 \quad (c>1)$