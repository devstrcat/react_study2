# react_hook
- Hook 이란?
 함수형 컴포넌트에서 React state와 생명주기 기능을 연동할 수 있게 해주는 함수

 ## 규칙
1. 리액트 훅(Hooks)은 반드시 최상위에서 호출해야 한다.
훅은 호출되는 순서에 의존하기 때문에 반복문이나 조건문, 중첩된 함수 내에 호출하면 안된다.
<br />
2. 리액트 훅(Hooks)은 함수형 컴포넌트 또는 커스텀 훅에서만 호출해야 한다.
훅은 일반 자바스크립트 파일에서는 사용할 수 없습니다. 반드시 리액트 내에서만 사용해야 한다.

## 종류 

1. useState 
- useState()는 상태 관리하는 훅으로, 가장 많이 사용하는 대표적인 훅 중 하나입니다. state는 숫자, 문자, boolean 뿐만 아니라 배열, 객체 등 여러 값으로 설정할 수 있다.
현재의 state 값과 state 값을 업데이트하는 함수를 반환하는 함수
<br />
- useState 함수는 배열을 반환하기 때문에 자바스크립트 표현식 중 하나인 '구조 분해 할당' 문법을 사용한다.

```
import React, { useState } from 'react';

const CountFunc = () => {
    const [count, setCount] = useState(0);

    return(
        <p>{count}</p>
        <button onClick={() => setCount(count + 1)}>버튼</button>
    )
}
```

1.1 상태관리 종류
1. Redux 
2. Recoil
3. MobX
4. Context API (useState, useReducer ...)

- 왜 Recoil을 사용해봐야하는가

    - 장점
    1. Redux,Recoil은 store 구성을 하는데 Redux는 구성이 어려운 경우가 많기도 하고 Recoil 구성을 위한 코드가 쉽다.
    <br />
    2. Context API와 달리 상태 변경시 컴포넌트를 리렌더링 하지 않고 필요한 부분만 업데이트 한다.
    <br />
    3. 기존의 자주 사용하는 useState 나 hook을 사용해봤다면 쉽게 익숙해질수 있다.

    - 단점
    1. 다른 상태관리 보다 늦게 개발되어 정보가 많은 편은 아니다.
    <br />
    2. 다른 라이브러리와의 호환성 문제가 나타날 수 있다.

1.2 useState와 useReducer 의 차이
- useReducer 도 useState처럼 상태를 관리하고 업데이트 할 수 있는 hook 이다.
<br />
- 차이점은 useState는 컴포넌트 내부에 상태 업데이트 로직이 존재하고 useReducer는 컴포넌트 외부에 상태 업데이트 로직을 작성할 수 있다는 것이다.
<br />
- 그렇게 되면 상태 업데이트 로직을 분리하여 컴포넌트 외부에 존재하면 코드 최적화도 되고 길어지면 파일로 분리를 시켜 분리된 파일을 불러와서 사용할 수 있게 된다.

```
// useState
import React, { useState } from 'react';

const CountFunc = () => {
    const [number, setNumber] = useState(0);

    const onDecrease = () => {
        setNumber((prevNumber) => prevNumber - 1);
    }
    const onIncrease = () => {
        setNumber((prevNumber) => prevNumber + 1);
    }

    return(
        <p>Count: {number}</p>
        <button onClick={onDecrease}>-버튼</button>
         <button onClick={onIncrease}>+버튼</button>
    )
}
```

```
// useReducer
import React, { useRecuder } from 'react';

function reducer(state, action) {
    switch(action.type) {
        case "decrement': 
            return state - 1;
        case "increment":
            return state + 1;
        default:
            throw new Error();   
    }
}

const CountFunc = () => {
    const [number, setNumber] = useReducer(0);

    return(
        <p>Count: {number}</p>
        <button onClick={onDecrease}>-버튼</button>
         <button onClick={onIncrease}>+버튼</button>
    )
}
```

2. useEffect
- Lifecycle(생명주기)와 관련이 있고 생명주기를 이용해 특정 작업을 실행할 수 있도록 하는 hook이다.
컴포넌트가 생성, 업데이트, 소멸되는 과정에서 컴포넌트가 렌더링될 때마다 실행된다.

2.1 useEffect 실행 조건 
-  마운트

    컴포넌트가 페이지에 처음 렌더링 된 후 useEffect는 ‘무조건’ 실행된다.
    <br />
-  업데이트

    - seEffect의 dependency array의  state 값이 변경 될 때
    - 부모가 리렌더링 될 때
    - context가 바뀔 때
    <br />
-  언마운트

    컴포넌트가 페이지에서 사라질 때
    <br />
- 기본문법
    ```
    - 렌더링 될 때마다 실행
    useEffect(() => { 작업내용 });

    - 렌더링 될 때 딱 한 번만 실행
    useEffect(() => { 작업내용 },[]);

    - 컴포넌트가 화면에 나타날 때(마운트) + dependency array의 값이 변경 될 때마다 실행
    useEffect(() => { 작업내용 },[value]);

    useEffect(() => { 
        
    });
    ```
    <br /><br />
- 예제
    - 렌더링 될때 마다 실행
    ```
    mport { useEffect, useState } from "react";

    function App() {
    const [count, setCount] = useState(1);
    const [name, setName] = useState("");

    const handleCountUpdate = () => {
        setCount(count + 1);
    };

    const handleChangeName = (e) => {
        setName(e.target.value);
    };

    useEffect(() => {
        console.log("렌더링");
    });

    useEffect(() => {});
    return (
        <div className="App">
        <div>
            <button onClick={handleCountUpdate}>Update</button>
            <p>count: {count}</p>
            <div>
            <input type="text" value={name} onChange={handleChangeName} />
            <p>Name: {name}</p>
            </div>
        </div>
        </div>
    );
    }

    export default App;
    ```
    - name,count 변경시 렌더링출력하려면
    ```
    import { useEffect, useState } from "react";

    function App() {
    const [count, setCount] = useState(1);
    const [name, setName] = useState("");

    const handleCountUpdate = () => {
        setCount(count + 1);
    };

    const handleChangeName = (e) => {
        setName(e.target.value);
    };

    useEffect(() => {
        console.log("name 렌더링1");
    }, [name]);
    useEffect(() => {
        console.log("count 렌더링2");
    }, [count]);

    useEffect(() => {});
    return (
        <div className="App">
        <div>
            <button onClick={handleCountUpdate}>Update</button>
            <p>count: {count}</p>
            <div>
            <input type="text" value={name} onChange={handleChangeName} />
            <p>Name: {name}</p>
            </div>
        </div>
        </div>
    );
    }

    export default App;
    ```
    - 마운팅 될 때 딱 한번 실행 - dependency array 에 빈 배열
    ```
    useEffect(() => {
    console.log("마운팅");
    }, []);
    ```

- 기본문법
    ```
    - 언마운트, 업데이트 직전 마다 실행
    useEffect(() => {
        ...실행내용
        return () => { 컴포넌트가 언마운트 될 때 실행 될 Clean-up 함수
        Clean-up 함수 실행 내용 (초기화)
        };
    });

    - 언마운트 될 때만 실행 - dependency array 에 빈 배열
    useEffect(() => {
        ...실행내용

        return () => { 컴포넌트가 언마운트 될 때 실행 될 Clean-up 함수
        Clean-up 함수 실행 내용 (초기화)
        };
    }, []);
    ```

- 예제
    - 언마운트 될 때만 실행
    ```
    Timer.js
    import { useEffect } from "react";

    const Timer = (props) => {
    useEffect(() => {
        console.log("타이머가 호출 되었군요!");
        const timer = setInterval(() => {
        console.log("타이머가 실행중...");
        }, 1000); // timer가 1초마다 실행

        return () => {
        clearInterval(timer);
        console.log("타이머가 종료 되었군요.");
        };
    }, []);
    return (
        <>
        <div>타이머를 시작합니다. 콘솔창을 보세요.</div>
        </>
    );
    };

    export default Timer;
    ```

    ```
    App.js
    import { useEffect, useState } from "react";
    import Timer from "./Timer";

    function App() {
    const [showTimer, setShowTimer] = useState(false);
    return (
        <div className="App">
        <button
            onClick={() => {
            setShowTimer(!showTimer);
            }}
        >
            Toggle Timer
        </button>
        {showTimer && <Timer />}
        </div>
    );
    }

    export default App;
    ```
