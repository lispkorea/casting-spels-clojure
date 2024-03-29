# 만들어진 세상 돌아다녀보기

- [원문](https://www.lisperati.com/walking.html)


 이제 게임 세상에 무엇이 있는지 알 수 있게 되었으니, 이 안에서 걸어 다닐 수 있는 코드를 작성해 보겠습니다.
 편의상 상태를 변경하는 (사이드 이펙트)함수 이름의 끝에 `!`를 붙이도록 하겠습니다.

 함수형 스타일이 아닌 사이드 이펙트를 지닌 `방향으로걷기!` 함수는 방향을 지정하고 그 방향으로 걸어갈 수 있게 해줍니다.
 
``` clojure
(defn 방향으로걷기! [방향]
  (let [다음으로 (->> atom_플레이어_현재장소
                      deref
                      상수_전체지도
                      :사전_경로
                      방향)]
    (if 다음으로
      (let [[_ 장소] 다음으로]
        (reset! atom_플레이어_현재장소 장소)
        (둘러보기))
      ["그쪽으로 갈 수 없습니다"])))
```

- `if` 함수는 조건이 참이면 첫 번째 매개변수를 실행하고 거짓이면 두 번째 매개변수를 실행합니다.
  - ex) `(if true "참" "거짓")` -> `"참"`

``` clojure
> (방향으로걷기! :서쪽)

["[아름다운 `정원`]: `우물`이 앞에 보인다"
 "`동쪽`으로 가는 문이 있다"
 "`개구리`(이/가) 바닦에 있다"
 "`사슬`(이/가) 바닦에 있다"]
```

 `방향으로걷기!`함수에선 키워드 `:서쪽`을 사용하였습니다. 사용자가 입력하기 편하게 하기 위해 키워드가 아닌 그냥 `서쪽`을 입력하면 어떨까요?
 코드 모드에서 `서쪽`을 입력하면 평가중에 서쪽이라는 변수가 없다고 오류가 발생할 것입니다. 그렇다고 데이터 모드로 바꾸려면 따옴표(`'`)를 사용해야 합니다.

 `:`의 입력을 피하니 `'`를 입력하라니... `'`없이 `서쪽`이라는 단어를 데이터로 사용할 방법은 없을까요?

## 짚고넘어갈것

- [if](https://clojuredocs.org/clojure.core/if)
