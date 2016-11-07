# 예외

## 초난감 예외처리

* 모든 예외는 적절하게 복구되던지, 작업을 중단시키고 운영자 또는 개발자에게 통보되어야 한다.
* 이런코드는 쓰지말자
```
} catch (Exception e) {
    e.printStackTrace();
}
```

* 굳이 예외를 잡아서 뭔가 조치할 방법이 없다면 잡지 말아야한다. 메소드에 throws를 선언해서 메소드를 밖으로 던지고 자신을 호출한 코드에 예외처리 책임을 전가하라.

##예외의 종류와 특징

###Error

* `java.lang.Error` 클래스의 서브클래스
* 시스템에서 뭔가 비정상적인 상황이 발생
* 주로 JVM이 뱉고 애플리케이션 코드에서 잡으려해봤자 무의미하다
* OutOfMemory를 어쩌게..

###Exception과 체크예외
* `java.lang.Exception` 클래스와 그 서브클래스
* 개발자들이 만든 애플리케이션 코드의 작업중에 예외상황이 발생했을 경우 사용
* 반드시 예외처리 구문과 함께 쓰여야 한다
* 예상치 못한 예외가 발생할 건덕지가 1이라도 있으면 체크예외!
* 즉 처리해서 예외를 복구할 건덕지도 1이라도 존재한다는걸 의미한다
* IOException과 SQLException

###RuntimeException과 언체크예외
* `java.lang.RuntimeException` 클래스를 상속한 예외
* 명시적인 예외처리를 강제하지 않기 때문에 언체크예외
* 코드에서 미리 조건을 체크하도록 주의깊게 만든다면 피할수 있어
* NullPointException과 IllegalException

##예외처리방법
###예외복구
* 예외상황을 파악하고 문제를 해결해서 정상상태로 돌려놓는것
* 그래서 처리됐으면 어플리케이션에서는 정상적인 흐름에 따라 처리되어야 한다
* 파일을 읽으려는데 파일이 없네? 뭐 이런거
* API를 사용하는 개발자로 하여금 예외가 발생했음을 미리 인지시켜주고, 적절한 처리를 할것을 강요

###예외처리회피
* 예외처리를 자신이 담당하지 않고 자신이 호출한 쪽으로 던져버리는것
* 콜백/템플릿 관계처럼 긴밀한 관계를 갖고 있는 쪽에서 다른 오브젝트에게 예외처리의 책임을 지게 하거나,
* 자신을 사용하는 쪽에서 예외를 처리하는것이 최선이라는 확신이 있어야해

###예외전환
* 발생한 예외를 그대로 던지는게 아닌 적절한 예외로 전환해서 던짐
* 내부에서 발생한 예외를 그대로 던지는것이 그 상황에 대한 적절한 의미를 부여해 주지 못하는경우, 그 의미를 분명하게 해줄수있는 예외로 바꾸기 위해 사용한다
* `SQLException`을 `DuplicationUserIdException` 으로 바꿔던진다던지..
* 또, 예외를 처리하기 쉽고 단순하게 하기 위해 사용한다
* 체크예외를 언체크예외로 포장해서 던진다던지..
* 생성자를 통해서 원래의 에러를 주입! `throw new DuplicationUserIdException(e);`

##예외처리전략
###런타임 예외의 보편화
* 자칫하면 throw Exception을 난발하게 돼 체크예외는
* 대응이 불가능한 체크예외라면 빨리 런타임 예외로 포장해서 던져버리는게 낫다
* 예전에는 복구할 가능성이 1이라도 있으면 체크예외로 만든다 생각했지만, 지금은 항상 복구할수 있는 예외가 아니라면 일단 언체크 예외로 던진다.
* 그걸 잡아서 처리하던지 흥- 이런느낌