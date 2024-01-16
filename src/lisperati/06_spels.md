# 스펠 외우기

- [원문](https://www.lisperati.com/spels.html)

자, 이제 리스프의 놀랍도록 강력한 기능을 배워보겠습니다: SPEL 만들기!

스펠(마법, `SPEL`)은 `S`emantic `P`rogram `E`nhancement `L`ogic의 줄임말로, :TODO 컴퓨터 코드의 세계 내부에 새로운 동작을 생성하여 필요에 맞게 동작을 사용자 정의하기 위해 기본적인 수준에서 Lisp 언어를 변경하는 것으로, 리스프에서 가장 마법처럼 보이는 부분입니다.
 스펠을 사용하려면 먼저 리스프 컴파일러 내에서 스펠을 활성화 시켜야 합니다.

``` clojure
(defmacro def-스펠 [& rest]
  `(defmacro ~@rest))
```

좋습니다. 이제 활성화 되었습니다. 이제 첫 스펠`이동`을 외워봅시다:

``` clojure
(def-스펠 이동 [방향]
  `(방향으로걷기! ~(keyword 방향)))
```

- `keyword`
  - ex) `(keyword '서쪽)` => `:서쪽`

 이 코드가 하는 일은 리스프 컴파일러에게 `이동`라는 단어가 실제로는 `방향으로걷기!`라는 단어이며, 내부적으로 인자로 들어온 방향(ex `서쪽`)을 키워드방향(ex `:서쪽`)으로 만들어 줍니다.
 코드를 완전히 컴파일하기 전에 다른 것으로 변경하는 특수한 코드를 프로그램과 컴파일러 사이에 몰래 삽입할 수 있습니다:

![](../res/spel_compile.jpg)

 리스프에서는 코드와 데이터가 거의 동일하게 보입니다. 매우 일관되고 깔끔한 디자인입니다! 새로운 스펠을 사용해 봅시다:

``` clojure
> (이동 서쪽)
;; (방향으로걷기! :서쪽)

["[아름다운 `정원`]: `우물`이 앞에 보인다"
 "`동쪽`으로 가는 문이 있다"
 "`개구리`(이/가) 바닦에 있다"
 "`사슬`(이/가) 바닦에 있다"]
```

훨씬 나아졌습니다!

이제 게임 세상에서 오브젝트를 집는 함수를 만들어 보도록 하겠습니다:


``` clojure
(defn 오브젝트-집기! [오브젝트]
  (let [오브젝트이름 (name 오브젝트)]
    (if-not (오브젝트가-해당-장소에있는가? @atom_플레이어_사전_오브젝트_랑_장소 오브젝트 @atom_플레이어_현재장소)
      [(format "여기에는 `%s`(이/가) 없습니다" 오브젝트이름)]
      (do
        (swap! atom_플레이어_사전_오브젝트_랑_장소 assoc 오브젝트 :주인공-인벤토리)
        [(format "`%s`(을/를) 집어들었습니다" 오브젝트이름)]))))
```

이 함수는 오브젝트가 실제로 현재 위치의 바닥에 있는지 확인하고, 만약 그렇다면 `atom_플레이어_사전_오브젝트_랑_장소`를 갱신하고, 성공 여부를 알려주는 문장을 반환합니다.
갱신할때 [앞서 배운](./03_data-cont.md) `swap!` 함수를 활용했습니다.
`atom_플레이어_사전_오브젝트_랑_장소`에 `assoc` 함수를 적용시키며, 인자로는 `오브젝트`와 `:주인공-인벤토리`를 넘겨주었습니다.

이 함수도 앞선 `방향으로걷기!`와 같이 키워드를 인자로 받기에, `이동` 스펠처럼 좀 더 쉽게 사용할 수 있는 다른 스펠을 시전해 보겠습니다:

``` clojure
(def-스펠 집어들기 [오브젝트]
  `(오브젝트-집기! ~(keyword 오브젝트)))
```

이제 새로운 스펠을 사용해 봅시다:

``` clojure
> (집어들기 위스키)

["`위스키병`(을/를) 집어들었습니다"]
```

이제 유용한 함수 몇개 더 추가해 보도록 하겠습니다. 우선 현재 플레이어가 가지고 있는 오브젝트 목록을 볼 수 있는 함수를 만들어 보겠습니다:

``` clojure
(defn 플레이어-오브젝트-리스트-가져오기 []
  (filter #(오브젝트가-해당-장소에있는가? @atom_플레이어_사전_오브젝트_랑_장소 % :주인공-인벤토리) 상수_리스트_모든오브젝트))
```

이제 플레이어가 특정 오브젝트를 가지고 있는지 알려주는 함수를 만들어 보겠습니다:

``` clojure
(defn 가지고있는가? [오브젝트]
  (->> (플레이어-오브젝트-리스트-가져오기)
       (some #{오브젝트})
       (some?)))
```

`some`과 `some?` 함수가 보입니다. 이 둘의 차이점은 무엇일까요?

- `some`: 함수를 콜렉션의 각 요소에 적용하여, 하나라도 `true`이면 참을 반환, 아니면 `nil`을 반환
  - ex) `(some even? '(1 2 3 4))` => true
- `some?`: nil이면 false, 아니면 true
  - ex) `(some? nil)` => `false`

some함수의 반환값이 `true/false`가 아닌 `true/nil`이기에, `some?`을 사용하여 `true/false`로 변환해주는 것입니다.

## 짚고넘어갈것

- [defmacro](https://clojuredocs.org/clojure.core/defmacro)
  - `'`: [quote](https://clojuredocs.org/clojure.core/quote)
  - \` : back-quoting
  - `~`: [unquote](https://clojuredocs.org/clojure.core/unquote)
  - `~@`: [unquote-splicing](https://clojuredocs.org/clojure.core/unquote-splicing)
- [keyword](https://clojuredocs.org/clojure.core/if)
- [do](https://clojuredocs.org/clojure.core/do)
- [assoc](https://clojuredocs.org/clojure.core/assoc)
- [dissoc](https://clojuredocs.org/clojure.core/dissoc)
- [some](https://clojuredocs.org/clojure.core/some)
- [some?](https://clojuredocs.org/clojure.core/some_q)