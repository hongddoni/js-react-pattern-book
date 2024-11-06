# 최신 자바스크립트 문법과 기능

### 5.1 애플리케이션 분리의 중요성

모듈형 자바스크립트는 앱을 모듈이란 단위로 쪼갤 수 있습니다.   
모듈은 다른 모듈을 가져올 수 있으며, 이 모듈이 또 다른 모듈을 가져올 수 있어 중첩된 모듈로 구성될 수 있습니다.
   
확장 가능한 자바스크립트 생태계에서 앱이 **모듈형**이란 것은 잘게 분리된 모듈로 구성돼있음을 뜻합니다.   
이렇게 이루워진 느슨한 결합은 **의존성**을 낮추어 앱의 유지보수를 용이하게 만듭니다.

AMD와 CommonJs 모듈은 초기 js에서 모듈화를 구현하기 위해 가장 많이 사용된 패턴입니다.   
모듈을 자연스레 가져올 수 없던 문제를 해결하기 위해 ES6 및 ES2015에서 모듈 관련 기능이 추가됐습니다.

### 5.2 모듈 가져오기와 내보내기
모듈을 사용하면 각 기능에 맞는 독립적인 단위로 코드를 분리할 수 있습니다.   
모듈형 언어가 되기 위해서는 의존성을 가진 모듈을 가져오고, 내보낼 수 있어야 합니다.   
이에 js 모듈은 es2015부터 import를 통해 가져오기를 export를 통해 내보내기를 할 수 있게 되었습니다.

- import 사용시 내보내기된 모듈을 지역변수로 가져올 수 있으며, 기존 변수명과 충돌을 피하고자 이름을 바꿔서 가져올 수 있습니다.
- export 사용시 지역 모듈을 외부에서 읽을 수는 있지만 수정할 수 없도록 만듭니다.   
    
Note   
    
    .mjs는 모듈 파일과 기존 스크립트(.js)를 구분하기 위해서 쓰이는 모듈 전용 확장자입니다. .mjs확장자를 사용하여 런타임 및 빌드도구에 모듈임을 알릴 수 있습니다.

일반적으로 모듈 파일은 여러 함수, 상수 및 변수를 갖고 있습니다. 파일 끝부분에서 내보내고 싶은 모듈을 객체로 정리하여 하나의 export 문으로 단번에 내보낼 수 있습니다.
```
파일명: staff.mjs

const baker ={...}
const pastryChecf = {...}

export { baker, pastryChef, assistant };
```

마찬가지로 사용할 모듈만 가져오는 것도 가능합니다.
```
import {baker} from "./modules/staff.mjs;
```

< script> 태그에서 type에 모듈을 명시하여 브라우저에게 알릴 수 있습니다.
```
<script type='module' src='main.mjs'></script>
<script nomodule src='fallback.js></script>
```
nomodule 속성은 브라우저에게 모듈이 아님을 알려줍니다.   
이 속성은 모듈 문법을 사용하지 않는 대체 스크립트에 유용하며 모듈을 지원하지 않는 브라우저에서도 기능이 제대로 작동할 수 있도록 합니다.

### 5.3 모듈 객체
모듈을 객체로 가져오면 모듈 리소스를 깔끔하게 가져올 수 있습니다.
```
import * as Staff from "...";

export const oven = {
    makeCupcakes(toppings) {
        Staff.baker.bake('cupcake', toppings);
    }
}

```

### 5.4 외부 소스로부터 가져오는 모듈
es2015부터는 외부 소스에서 가져오는 원격 모듈을 쉽게 가져올 수 있게 되었습니다.

```
import {cakeFactory} from "https://example.com/.../modules.mjs";
cakeFactory.oven.makeCupcake('...');
```

### 5.5 정적으로 모듈 가져오기
앞 예시에서 보여준 건 정적 가져오기라고 합니다.   
정적 가져오기는 메인 실행 전에 모듈을 다운로드하고 실행해야 합니다.   
따라서 초기 페이지 로드 시 많은 코드를 미리 로드해야 하므로 성능에 문제가 생길 수도 있습니다.

### 5.6 동적으로 모듈 가져오기
모듈은 초기에 모두 미리 로드하기보다는 필요한 시점에만 로드하는 것이 더 이로울 때가 있습니다.   
지연 로딩(lazy-loading) 모듈을 사용하면 필요한 시점에 로드할 수 있습니다.   
예를 들어 사용자가 링크나 버튼을 클릭할 때 로드하게 만들 수 있어 초기 로딩 시간을 줄일 수 있습니다. 이게 동적 가져오기가 생겨난 이유입니다.  

동적 가져오기는 함수와 비슷한 새로운 형태의 가져오기입니다.  
import(url)는 요청된 모듈의 네임스페이스 객체에 대한 프로미스 객체를 반환합니다.  
이 프로미스 객체는 모듈 자체와 모든 모듈 의존성을 가져온 후, 인스턴스화하고 평가한 뒤에 만들어집니다.
```
form.addEventListener("submit", e => {
    e.preventDefault();
    import("url")
        .then(module => {
            module.oven.makeCupcake(var)
        })
})
```

동적 가져오기는 await과 함께 사용할 수 있습니다.
```
let module = await import("url");
```
동적 가져오기를 사용하면 모듈이 사용될 때만 다운로드되고 실행됩니다.   
사용자 인터랙션에 반응하거나 화면에 로드시 실행하기 등 자주 사용되는 패턴은 동적 가져오기를 통해 바닐라 js에서도 쉽게 구현할 수 있습니다.

### 5.6.1 사용자 상호작용에 따라 가져오기
사용자의 이벤트 호출 후 로드가 필요할 때가 있습니다.  
동적 가져오기를 활용하면, 실행한 다음에 따라오는 함수를 통해 원하는 기능을 사용할 수 있습니다.

다음 예시는 lodash.sortby 모듈을 동적으로 로드하여 정렬 기능을 구현하는 코드입니다.
```
const btn = document.querySelector('button');
btn.addEventListener('click', e=>{
    e.preventDefault();
    import('loadash.sortby')
        .then(module => module.default)
        .then(sortInput())
        .catch(err => {console.log(err)});
});
```

### 5.6.2 화면에 보이면 가져오기
모듈을 지연 로딩으로 구현하면 좋습니다.   
IntersectionObserver API를 사용하면 컴포넌트가 화면에 보이는지 감지할 수 있고, 이에 따라 모듈을 동적으로 로드할 수 있습니다.

### 5.7 서버에서 모듈 사용하기
Node 15.3.0 이상 버전에선 js 모듈을 지원하고 npm 패키지 생태계와 호환됩니다.   
Node는 type이 module이라면 .mjs와 .js로 끝나는 파일을 js 모듈로 취급합니다.
(여기서 type은 package에 정의된 타입)

### 5.8 모듈을 사용하면 생기는 이점
- 한 번만 실행된다.
    - 기존 js는 DOM에 추가될 때마다 실행되는 반면, 모듈은 한 번만 실행됩니다. **JS 모듈을 사용하면 의존성 트리의 가장 내부에 위치한 모듈이 먼저 실행됩니다.** 가장 내부에 위치한 모듈이 먼저 평가되고 여기에 의존하는 모듈에 접근할 수 있다는 것이 이점입니다.
- 자동으로 지연 로드된다.
- 유지보수와 재사용이 쉽다.
- 네임스페이스를 제공한다.
    - 글로벌 네임스페이스를 오염시키지 않고 지역적이다.
- 사용하지 않는 코드를 제거한다.
    - 번들러를 사용해서 트리쉐이킹을 가능하게 한다.

### 5.9 생성자, 게터, 세터를 가진 클래스
ES2015+에서는 모듈뿐만 아니라 생성자와 내부를 숨기는 기능을 가진 class가 추가되었습니다.

```
class Cake {
    constructor(name, toppings, price, cakeSize) {
        this.name =name;
        ...
    }

    // ES 2015 버전 이상에서는 모든 것을 함수를 피하고자 새로운 식별자를 사용하려고 했습니다.

    addToping ( topping ) {
        this.toppings.push(toping);
    }

    // 게터는 메서드 이름 앞에 넣어 사용합니다.
    get allTopings() {
        return this.toppings;
    }

    get qualifiesForDiscount() {
        return this.price > 5;
    }

    // 세터도 메서드 이름 앞에 넣어 사용합니다.

    set size(size) {
        if (size < 0) {
            throw new Error(...)
        }
        this.cakeSize = size;
    }
}

// 사용법
let cake = new Cake (...);
```

자바스크립트의 클래스는 prototype을 기반으로 하고 있으며, 사용하기 전에 미리 정의해야 합니다.  
extends 키워드를 통해 클래스를 상속받을 수도 있습니다.
```
class BirthdayCake extends Cake {
    surprise() {
        console.log('');
    }
}

let birthdayCake = new BirthdayCake(...);
birthdayCake.surprise();
```

앞에서 봤던 예제를 잘 살펴보면 function 단어가 없다는 걸 깨달으실 겁니다.  TC39에서는 function의 남용을 막고자 했습니다.  

또한 super 키워드도 지원합니다. 이는 **자기상속패턴**을 사용할 때 유용합니다.
```예제생략```

최신 js 클래스는 내부 멤버를 비공개로 정의할 수 있습니다.  
클래스 멤버는 기본적으로 공개 상태이며, # 를 앞에 붙여 비공개 멤버로 만들 수 있습니다.
```
class Temp {
    #privateField;
}
```
js class는 static 키워드를 통해 정적 메서드와 프로퍼티를 정의할 수 있습니다. 정적 멤버 클래스를 초기화하지 않고 사용할 수 있습니다. 주로 설정이나 캐시 데이터를 보관하기 위해 사용됩니다.

```
class Cookie {
    constructor(flavor) {
        ...
    }

    static brandName = 'good'
}
console.log(Cookie.brandName) // good 출력
```

### 5.10 자바스크립트 프레임워크와 클래스
클래스를 대체하기 위한 것들이 라이브러리와 프레임워크에서 많이 시도되었고  
리액트 Hooks는 클래스를 사용하지 않고 리액트의 상태와 라이프사이클을 다룰 수 있도록 만들어 졌습니다.