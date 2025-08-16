# rust-lang-cargo-test

## Rsutとは
- Rsutは，メモリ安全性，高速性，並行性を重視して設計されたシステムプログラミング
- c++に匹敵するパフォーマンスを持ちながら**所有権(Ownership)**という独自の概念でメモリ安全性を保障する．これによりガベージコレクションなしでNullポインタ，Use-after-freeなどのバグを防ぐ

## 主要な特徴と文法
1. 所有権(Ownership)
    - メモリ管理の安全性を確保するためのルール
    - 基本原則
        - 各値には，1つの所有者がいる
        - 一度に1つの所有者しかいない
        - 所有者がスコープを抜けると，値は自動的に解放される
    ```rs
    let s1 = String::from("hello");
    let s2 = s1; // <- s1の所有権がs2に移動，s1は無効になる
    print!("{}", s1); // これはエラー
    print!("{}", s2); // これはOK
    ```

2. 借用(Borrowing)
    - 所有権を移動させずに，一時的に値を使う
    - 参照(reference)を使う
        - 不変の借用(&): 複数の不変な参照を同時に持つことができる
        - 可変の借用(&mut): 一度に1つの科変な参照しか持つことができない．これにより複数の参照が同時に値を変更するデータ競合を防ぐ
    - 参照による借用
        ```rs
        fn main()
        {
            let mut s = String::from("hello");
            change_string(&mut s);
            print!("{}", s); // ==> hello world
        }

        fn change_string(some_string: &mut String)
        {
            some_string.push_str("world");
        }
        ```
- 主なデータ型
    - プリミティブ型
        - 整数型 | i32, u64 | 符号付き(なし)整数．iが符号付き，uが符号なし
        - 不動小数点型 | f64 | f32とf64．デフォルトはf64
        - 論理型 | bool | trueまたはfalse
        - 文字列型 | char |　Unicodeのスカラー値を表す．シングルクォートで定義．'a'
    - コレクション型
        - タプル | (1, 2.0, 'h') | 複数の型を組み合わせた固定長の値
        - 配列 | [1, 2, 3] | 同じ型の要素をまとめた固定長のコレクション
        - ベクトル | Vec<T> | 実行時にサイズが変化する動的配列
        - 文字列 | 
    - 辞書型
        - HashMap | hashMap<Key,Value> | キーと値のペアを保存するコレクション `std::collections::HashMap`
- 関数と制御フロー
    - 関数: `fn`キーワードで定義．引数と戻り値の型を明示的に指定する
        ```rs
        fn add(x: i32, y: i32) -> i32
        {
            x + y // 最後の式が自動的に戻り値になる
        }
        ```
    - `if`式: 条件式として値を返すことも可
        ```rs
        let condition = true;
        let number = if condition { 5 } else { 6 };
        ```
    - `match`式: パターンマッチングを行い複数の分岐を簡潔に書ける
        ```rs
        let number = 13;
        match number
        {
            1 => println!("One"),
            2..=10 => println!("Two to Ten"),
            _ => println!("Other"),
        }
        ```
- ループ: 
    - `loop`: 無限ループ．`break`でぬける
    - `while`: 条件が`true`の間繰り返す
    - `for`: イテレータを反復処理する
