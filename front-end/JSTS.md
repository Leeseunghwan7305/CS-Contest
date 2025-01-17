1. **JavaScript에서 호이스팅(hoisting)이란 무엇인가요? `var`, `let`, `const` 선언이 호이스팅에서 어떻게 다르게 동작하는지 설명해주세요. 또한, 함수 선언과 함수 표현식에서의 호이스팅 차이점에 대해서도 설명해주세요.**

- 답 : 호이스팅이란 자바스크립트 엔진이 실행되기 전 원활한 환경을 만들기위해 변수나 함수 선언문의 메모리 공간을 미리 할당하는 것을 의미합니다.

      var, let, const는 모두 호이스팅이 된다는 공통점을 가지고 있습니다. 하지만 es5문법인 var와 es6문법인 let,const는 각각 다르게 동작합니다.
      var는 호이스팅이 됐을때 선언문의 초기값으로 undefined이 할당됩니다.
      let은 호이스팅이 됐을때 선언문의 초기값으로 어떠한 값도 들어가지않습니다.
      const는 상수선언문이기떄문에 선언문과 할당값이 같이 들어가야합니다. 그렇기떄문에 let과 같이 호이스팅이 되어도 할당전에는 에러가 납니다.

      함수선언문과 함수표현식은 호이스팅되는 범위에 대한 차이가 있습니다.
      함수선언문은 함수의 모든 범위가 호이스팅됩니다.
      함수표현식은 함수의 선언부만 호이스팅됩니다.

<br>

---

2. **JavaScript에서 이벤트 루프가 비동기 작업을 어떻게 처리하는지 설명해주세요. 특히 마이크로 큐와 태스크 큐가 어떻게 관리되는지와, 이 두 큐에서 Promise와 setTimeout의 처리 우선순위에 대해서도 함께 설명해주세요.**

- 답 : 이벤트 루프에서 사용되는 것은 크게 자바스크립트 엔진과 웹 브라우저 내부에 멀티쓰레드를 사용하는 WEB API가 있습니다.
  자바스크립트 엔진에는 힙과 콜 스택이 있고 web api에는 테스크큐와 마이크로 테스크큐가 있습니다.
  자바스크립트가 실행되면 정적데이터는 콜 스택에, 동적 데이터는 힙에 들어갑니다.
  그런 데이터 중에 무거운 데이터 (이벤트 , api , 애니메이션 등)은 바로 콜스택에 들어가지않고 webapi의 백그라운드에서 따로 실행하게 됩니다.
  이런 무거운 비동기 요청들은 완료되면 각각의 유형에 맞는 큐에 들어가서 대기하게 됩니다. 마이크로 큐는 promise같은 우선적으로 동작하는 유형이 들어가고, 테스크큐는 settimeout,fetch, addEventListener과 같이 콜백함수가 들어가는 데이터가 테스트큐에 들어갑니다.

      자바스크립트는 위의 첫줄부터 한줄한줄 읽게되는데 콜스택에 정적데이터가 쌓이고 그런 콜스택에 남은 요소가 없을 때 마이크로 테스트큐와 테스크큐에서 완료된 비동기처리 데이터들이 콜스택에 들어가 호출이 되기 시작합니다.
      promise와 settiemout의 처리 우선순위는 위의 언급과 같이 마이크로 큐는 우선적으로 동작하는데 promise는 마이크로 큐에 settimeout은 테스트큐에 들어가는 비동기유형이므로 promise와 settimeout이 같이 출력을 해도 promise가 먼저 출력이 됩니다.

<br>

---

3. **`this` 키워드가 JavaScript에서 어떻게 결정되는지 설명하고, 다양한 상황에서 `this`가 어떻게 바인딩되는지 예를 들어 설명해주세요.**

- 답 : this는 자신을 가리키는 참조변수입니다.
  this는 상황에 따라 값이 달라지는데
  첫번째로 값은 부모 object를 가리킵니다.
  javascript의 코드는 object안에 감싸져있고 그 object의 이름은 window입니다.
  그렇기 때문에 단순히 cosnole.log(this)를 입력한다면 window가 나오게 됩니다.

       두번째로 만든 object안에서 쓰는 this입니다. const cs대회:{
                                                      console.log(this);
                                                        }
      를 했을 때 값은 첫번쨰의 예와 같이 부모 object 즉 cs대회안에있는 데이터가 출력되게 됩니다.

      세번째는 이벤트 핸들러안에서 쓰는 this가 있습니다.
      이벤트 핸들러안에서 this를 쓰면 기본적으로 e.target값이 출력되지만 화살표함수를 썻다면 그 위의 객체인 window객체가 출력되게 됩니다.
      const handler=(e)=>{
        console.log(this) // window
      }
       const handler(e){
        console.log(this) // e.target
      }
      마지막으로 함수안에서 this를 쓰는 것입니다.
      첫번쨰 두번째와 마찬가지로 가장 가까운 부모 object를 찾아갑니다 . 즉 함수안에서 쓴 this는 window객체를 의미합니다.

<br>

---

4. **JavaScript의 비동기 처리 방식인 콜백, 프로미스, async/await의 차이점은 무엇인가요? 각 방식의 기본 개념과 장단점을 설명해주세요.**

- 답 :callback은 함수안에 함수를 넣는 방식입니다. 콜백의 장점은 동기적인 코드를 비동기적으로 동작할 수 있게 해줍니다. 하지만 콜백지옥이라고 콜백안에 콜백을 계속 넣으면서
  가독성을 떨어뜨리는 현상이 일어날 수 있습니다.

  promise는 콜백의 비동기적으로 동작할 수 있는 장점을 가져오고 콜백지옥이라는 단점을 보안한 문법입니다. 성공, 대기, 실패로 나누어서 결과값이 정해지고 then, catch, finally로 연장된 비동기 요청을 보낼 수 있습니다. 콜백보다는 가독성이 눈에 띄게 좋은 장점을 가지고 있습니다. 또한 promise.all과 같이 여러 비동기요청을 병렬적으로 요청할 수 있다는 점도 장점으로 가지고 있습니다. 하지만 계속되는 프로미스 체이닝이 지속된다면 콜백과 마찬가지로 가독성이 좋지않다는 단점은 계속 가지고 있습니다.

  마지막으로 async/await입니다 Es6문법으로 비교적 최근에 나온 비동기 처리 문법입니다. async/await의 내부 구현은 Promise로 이루어져있지만 차이점은 try/catch로 성공 실패가 가려지고 await만 붙여주면 비동기 처리가 되는 등 굉장히 개발자가 직관적으로 코드를 바라볼 수 있게 설계되어 있습니다.
  단점이라고 한다면 async await은 promise.all처럼 병렬처리가 되지 않아서 성능이 상황에 따라 안좋을 수가 있다.
