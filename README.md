<a name ="top"></a>
# struct-union

# 5.構造体、共用体

- 理解：構造体、共用体はどんなものか？
- 理解：構造体、共用体ではどのようにメモリが使用されるか？アラインメントとは？
- 考察：ゲームで構造体、共用体はどう使えそうか？、その注意点など

## 構造体
**構造体**は1個以上の変数をひとかたまりにまとめた型であり、構造体に含まれる1つ1つの変数はメンバや要素などと呼ばれる。

構造体はint型やdouble型などと同様に型の種類であり、構造体型と呼ぶ。intなどの基本的な型と大きく違うのは型の詳細を決めるのがプログラマーの役目
であるという点。つまり、構造体を使うにはまず、構造体型を定義し、そのあとその型の変数を定義するという流れになる。

配列と構造体の代表的な違いは以下の点
- 配列の要素はすべて同じ型であるが、構造体の要素はそれぞれ異なっていてもよい
- 配列の要素は添え字を使ってアクセスするが、構造体には添え字がない
- 配列の要素は隙間なく連続的に並んでいるが、構造体の要素はその保証がない

### 構造体の定義

```
// 生徒のデータ
struct Student_tag {
    char  name[32];                 // 名前
    int   grade;                    // 学年
    int   class;                    // 所属クラス
    int   score;                    // 得点
};
```
structは構造体を意味するワード。

タグ名（Student_tag)には、つける名前を記述する。複数の構造体を区別するために使う名前。タグは使うことがなければ省略できる。

各メンバの宣言では、いつもの変数宣言と同じで、型名と名前を書けばよい。初期値を指定することはできない。また、staticや、externを付加することもできない。
なお、この構造体型に所属するメンバの名前なので、他の場所で宣言されている名前と区別がつけられるため、他の名前と被っても問題ない。

定義は関数定義の外側にもどこかのブロックの内側にも記述できる。構造体型の変数を定義するときは、その位置から構造体型の定義が見えていなければならない。ブロック内で定義した場合
そのブロックの外側から使用することはできない。

複数のソースファイルから使いたい場合、ヘッダファイルに記述し、#includeで取り込める。

### 構造体変数を宣言
構造体型の定義が出来たら、構造体変数が宣言可能。```struct Student student;```

いつものように、変数studentが自動記憶域期間を持つのなら、各メンバは初期化されない。静的記憶域期間を持つのならば0に相当する値で初期化される。
# 注意
auto? static?extern?

メンバに初期値を与えるためには、構造体変数を定義するときに、まとめて初期化子を与える。
```struct タグ名 変数名 = {初期化子並び};```

```{}```の内側に、1つ以上の初期化子を```,```で区切って並べる。1つ目の初期化子は1つ目のメンバに2つ目の初期化子は2つ目のメンバに適応される。

配列と同様に、初期化子の個数が実際のメンバの個数よりも少ない場合、残りのメンバにはデフォルトの初期化子が与えられる。デフォルトの初期化子は、
静的記憶域期間の変数に与えられるデフォルトの初期値の規則と同様で、おおざっぱに言えば0である。初期化子のほうが多い場合はコンパイルエラーとなる。


可能であれば、初期化方法を使って初期化子を入力した方が安全であるが、後からメンバを増やしたり減らしたりすると、初期値を与える相手がいつの間にか変わってしまう可能性があることに注意。
この問題の対処として**要旨指示子**というものがある。これは特定の要素を選択的に初期化できる機能である。

#### 構造体の定義と構造体変数の宣言をまとめて行う
構造体型の定義とその型の変数の宣言をまとめて書くことができる。初期足の有無は自由。

```
struct {
    char  name[32];                 // 名前
    int   grade;                    // 学年
    int   class;                    // 所属クラス
    int   score;                    // 得点
} student = {"Saitou Takashi", 2, 3, 80};
```
このコードで構造体変数を定義できるので、以降のコードにこの構造体が登場しない可能性がある。
タグ名を省略しても構わないが、続きのコードで構造体型の名前を記述する箇所があるのならば、省略は不可。

### メンバにアクセス

メンバ変数へのアクセスは**構造体変数名.メンバ名**でアクセスを行う。
配列の場合は、各要素へのアクセスに添え字を使っていたが、構造体の場合には、```.```で表現される**メンバアクセス演算子**を使用する。
単に**ドット演算子**と呼ぶこともある。

できるだけ、変数が未初期化な瞬間が少なくなることが望ましいので、可能なら変数定義と同時にすべてのメンバを初期化するとよい。
正しい値で初期化しないとしても、いったん0で初期化しておくのは1つの手である。
```{}```の内側には最低1つの初期化子が必要であるが、後は省略しても自動的に0が入るので次にように書けばすべてのメンバ変数を0に相当する値で初期化できる。

```
struct Student_tag studen={0};
```

### 構造体型の配列
```
#include <stdio.h>

#define STUDENT_NAME_LEN 32         // 生徒の名前データの最大長

// 生徒のデータ
struct Student_tag {
    char  name[STUDENT_NAME_LEN];   // 名前
    int   grade;                    // 学年
    int   class;                    // 所属クラス
    int   score;                    // 得点
};

int main(void)
{
    struct Student_tag students[] = {
        {"Saitou Takashi", 2, 3, 80},
        {"Suzuki Yuji ", 1, 1, 67},
        {"Itou Miyuki", 3, 1, 79},
    };

    const size_t size = sizeof(students) / sizeof(students[0]);
    for (size_t i = 0; i < size; ++i) {
        printf("name: %s\n", students[i].name);
        printf("grade: %d\n", students[i].grade);
        printf("class: %d\n", students[i].class);
        printf("score: %d\n", students[i].score);
    }
}
```
構造体の配列を定義することも可能。
配列1つ1つの要素の型が、struct Student_tagということになる。配列の初期化のための```{}```の内側には、各々のstruct Student_tagを初期化するための```{}```が入る。

配列内の構造体のメンバにアクセスするときには、```student[i].name```のように、まず配列内の1要素を添え字演算子によって特定してから、メンバアクセス演算子を使って
メンバにアクセスするという流れになる。

### typedef

### 比較
### 要旨指示子
### 複合リテラル
### パディング
### 自己参照
### 構造体の入れ子


## 共用体

[上部へ移動](#top)
