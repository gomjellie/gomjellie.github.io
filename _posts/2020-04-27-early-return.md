---
layout: post
title:  "early return을 사용하자 (else혐오자가 사는법)"
date:   2020-04-27 16:45:00 +0900
categories: [패턴]
---

나는 else가 싫다. 뭔가 else if, else if 조건을 계속달면 생각이 복잡해지는 느낌이라고 해야되나. 아무튼 그냥 싫다.... 진짜 진짜 싫어해서 else를 안쓰고 프로그래밍하는 것을 선호한다. 오늘은 else를 안쓰고 프로그래밍 하는 방법에 대해서 알아본다!

## early return이 뭔데?

`early return`이란, return을 빨리 해버려서 뒷 코드의 구조를 단순하게 만들어주는 패턴이다. 정확히는 else를 제거하는것이 목적이다.

예를 들자면, 아래의 코드는 early return이 적용되지 않았다.

```c
int get_string_type(char* string) {
    int ret = 0;
    if (is_rule_1(string)) {
        ret = 1;
    } else {
        if (is_rule_2(string)) {
            ret = 2;
        } else {
            ret = 0;
        }
    }

    return ret;
}
```

아래는 early return으로 정리된 코드이다.
```c
int is_valid(char* string) {
    if (is_rule_1(string))
        return 1;
    if (is_rule_2(string))
        return 2;

    return 0;
}
```

함수 내에서 return을 하면 뒤의 코드로 진입하지 않는 특성을 이용해서 else { }로 구성되는 하나의 `brace block을 작성하지 않아도 된다!`

위의 예시는 간단하게 보여주기위해서 `early return`을 사용하지 않았을때 얼마나 코드가 복잡해 질 수 있는지를 다 표현하지 못했지만, 실제 코드에서 `is_rule_1`, `is_rule_2` 함수가 반복문으로 내부에서 구현되고 { } 스코프 안에 또 { } 그안에 또다시 { }.... 이렇게 `brace block 구조가 깊어지고` 가독성이 떨어지는것을 생각하면 early return을 사용하는것이 얼마나 코드를 간결하게 바꿔주는지 알 수 있을 것이다.

## early return에서 더 나아가서

위의 예시를 통해서 `early return`이 코드를 어떻게 단순하게 만드는지 알아보았다. 더나아가서 `early return`과 비슷한 패턴들을 소개하려고 한다.

`continue`, `break`를 통해서도 return처럼 else를 제거 할 수있는데, 이 경우에는 return과 다르게 함수가 종료되지 않기 때문에 더 넓게 활용가능하다.

![Image Alt 자세한 설명은 생략한다](/static/img/posts/common/king-sungmo.jpg)

## early return은 필수적인가?

아, 물론 early return 패턴을 사용하고 안하고는 개인의 취향에 달렸다.

때로는 if else 구조가 로직을 더 명시적으로 보여줄 수도 있다. (개인차가 있을 수 있다)
