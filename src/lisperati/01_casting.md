# Casting SPELs (Clojure)

- [원문](https://www.lisperati.com/casting.html)

 리스프(Lisp, 리슾)를 아는 사람이라면, 리스프가 다른 프로그래밍 언어들과 매우 다르다고 말할 것입니다.
 이는 실로 놀라울 정도로 많이 다릅니다. 이 만화를 통해 리스프가 얼마만큼 강력한지, 리스프의 독특한 디자인을 통해 배워보도록 하겠습니다.

![](../res/different.jpg)

리스프 언어에는 여러가지 종류가 있는데, 그 중 클로저로 진행하도록 하겠습니다.

`clj` 명령어를 통해 클로저의 REPL을 실행합니다.

``` clojure
$ clj
Clojure 1.11.1
user=>
```

REPL은 `R`ead `E`val `P`rint `L`oop의 약자입니다. REPL은 사용자가 입력한 코드를 읽고, 실행하고, 결과를 출력하고, 다시 반복합니다.

``` clojure
user=> (+ 1 2)
3
user=>
```

`user=>`에서 `user`는 현재 네임스페이스의 이름입니다. 네임스페이스안에서 정의한 함수/변수들은 네임스페이스에 저장됩니다. `ns`는 `n`ame`s`pace의 약자이며, 네임스페이스를 바꿀 수 있는 매크로 입니다.

 다음과 같이 `(ns spel)`로 네임스페이스를 `spel`로 바꿀 수 있습니다.

``` clojure
user=> (ns spel)
nil
spel=>
```

REPL을 종료하려면

- Window에서는 Ctrl-z를
- macOs/Linux에서는 Ctrl-d를 누르면 됩니다.


## 짚고넘어갈것

- [클로저(Clojure)](https://clojure.org)
- clj 명령어
- REPL 종료법