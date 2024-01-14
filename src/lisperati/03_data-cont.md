# 게임 세상을 위한 데이타 정의하기 - 계속

앞서 `def`를 이용해서 정의한 변수는 상수라고 했습니다. 즉, 한번 정의되면 그 값을 바꿀 수 없습니다.

그런데 게임에서는 상수가 아닌 변수가 필요합니다. 예를 들어 플레이어가 어떤 물건을 들고 있을때, 그 물건을 다른 물건으로 바꾸거나, 물건을 버릴 수 있어야 합니다.

클로저에는 이렇게 상태를 가지는 변수를 지정 할 수 있는 여러가지 타입들을 제공합니다. 그중에서도 가장 간단한 `atom`을 이용해서 변수를 정의해 보겠습니다.

편의상 `atom`변수는 `atom_`를 붙여서 정의하도록 하겠습니다.

``` clojure
;; 변수.
(def atom_플레이어_사전_오브젝트_랑_장소
  (atom 상수_사전_오브젝트_랑_장소))

(def atom_플레이어_현재장소
  (atom 상수_플레이어_시작_장소))

(def atom_플레이어_사슬을_용접하였는가
  (atom false))

(def atom_플레이어_양동이를_채웠는가
  (atom false))
```

아래는 `atom`의 간단한 사용법입니다.

``` clojure
> (def atom_테스트 (atom 1))
> atom_테스트
#object[clojure.lang.Atom 0x4aac85fa {:status :ready, :val 1}]

;; atom의 값을 가져오려면 deref를 사용
> (deref atom_테스트)
=> 1

;; deref와 @는 같은 의미
> @atom_테스트
=> 1

;; 강제로 셋팅
> (reset! atom_테스트 2)
> @atom_테스트
=> 2

;; 함수 적용
> (swap! atom_테스트 + 1 2 3)
> @atom_테스트
=> 8
```

이제 플레이어 정보를 초기화 시켜주는 함수 역시 정의해 보겠습니다.

``` clojure
(defn 초기화 []
  (reset! atom_플레이어_사전_오브젝트_랑_장소 상수_사전_오브젝트_랑_장소)
  (reset! atom_플레이어_현재장소              상수_플레이어_시작_장소)
  (reset! atom_플레이어_사슬을_용접하였는가   false)
  (reset! atom_플레이어_양동이를_채웠는가     false))
```

 `defn`은 `define function`의 약자로 함수를 정의합니다. 여기서는 아무런 인자도 받지 않고 있습니다. `초기화`함수는 플레이어의 atom변수들을 초기화하는 역할을 합니다.


## 짚고넘어갈것

- [atom](https://clojuredocs.org/clojure.core/atom)
- [deref](https://clojuredocs.org/clojure.core/deref), @
- [reset!](https://clojuredocs.org/clojure.core/reset!)
- [swap!](https://clojuredocs.org/clojure.core/swap!)
- [defn](https://clojuredocs.org/clojure.core/defn)

## 참고

- <https://clojure.wladyka.eu/posts/share-state/>