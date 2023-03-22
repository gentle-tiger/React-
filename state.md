# state

![스크린샷 2023-03-22 055355.png](state(%E1%84%92%E1%85%A1%E1%86%AB%E1%84%87%E1%85%A5%E1%86%AB%E1%84%83%E1%85%A5%E1%84%87%E1%85%A9%E1%84%80%E1%85%B5)%20bffad04fde494c458fd5f150d31f5804/%25EC%258A%25A4%25ED%2581%25AC%25EB%25A6%25B0%25EC%2583%25B7_2023-03-22_055355.png)

<aside>
💡 prop을 통해서 입력된 데이터를 우리가 만든 컴포넌트 함수가 처리해서 retrun 값을 만들면, 그 return 값이 새로운 ui가 된다. 컴포넌드 함수를 다시 실행해서 새로운 리턴값을 만들어 주는 또 하나의 데이터가 있는데, 그것이 state이다. 

prop과 state의 공통점은 **값이 변경되면 새로운 리턴 값을 만들어서 UI를 바꾼다는 것이고,** 차이점은 **prop은 컴포넌트를 사용하는 외부자를 위한 데이터**이며, **state는 컴포넌트를 만드는 내부자를 위한 데이터**이다.

</aside>

**list를 클릭했을 때 welcome의 내용이 바뀌게 하려면 어떻게 해야할까?** 

mode라는 지역변수의 값이 WELCOM이냐, READ냐에 따라서 본문의 값이 달라지게 하고 싶다면? 

- content라는 변수를 통해서 그 값을 출력되게 한다. 
그리고 그 자리에는 {content}를 넣는다.

```jsx
function App() {
  const mode ='WELCOME'
  const topics = [
    {id:1, title:'html', body:'html is ...'},
    {id:2, title:'css', body:'css is ...'},
    {id:3, title:'javascript', body:'javascript is ...'}
  ]
  let content = null;
  if(mode === 'WELCOME'){
    content = <Article title="WELCOME" body="Hello.WEB"></Article>
  }else if(mode === "READ"){
    content = <Article title="READ" body="Hello.READ"></Article>
  }
  return (
    <div>
        <Header title="WEB" onChangeMode={()=>{
          alert('Hi');
        }}></Header>
        <Nav topics={topics} onChangeMode={(id)=>{
          alert(id);
        }}></Nav>
        {content}
    </div>
  );
  }
export default App;
```

- mode의 값에 따라 내용을 변경하는 event를 발생시키려면 어떻게 해야할까?
- mode의 값이 변하면 App()이 다시 리로드 되면서 값이 변하도록 하는 것.

```jsx
console.log('_mode',_mode);

> _mode (2) ['WELCOME', ƒ]
				0: "WELCOME" /* 0번쨰 원소는 welcome이다. 상태의 값을 읽을 때 사용*/
				1: ƒ ()      /* 1번째 원소는 함수이다. 상태의 값을 변경할 때 사용하는 함수*/
				length: 2   
				[[Prototype]]: Array(0)

-------
const _mode = useState('WELCOME')
const _mode = _mode[0]; /* 라는 것은 상태의 값을 읽을 수 있다는 것을 의미한다. */
const setMode = _mode[1]; /* 1번째 mode 인 setMode를 통해서 mode의 값을 바꿀 수 있다는 규칙이 있는 것이다. */
/*
useState의 인자값은 그 스테이트에 초기 값이다. 
스테이트의 값은 0번째 인덱스의 값으로 읽는다. 
스테이트를 바꿀 때는 1번째 인덱스의 값으로 함수로 바꾼다.
```

```jsx
/*실제로 사용할 때는 이렇게 한다. 위의 세 줄이 아래의 세줄과 똑같은 코드이다. */
const [mode, setMode] = useState('WELCOME');
```

- mode는 상태의 값을 읽을 때 사용하고, setMode는 그 값을 변경할 때 사용한다.

```jsx
function App() {
  **const [mode, setMode] = useState('WELCOME')**;/*지역변수를 state상태로 업그레이드*/
  const topics = [
    {id:1, title:'html', body:'html is ...'},
    {id:2, title:'css', body:'css is ...'},
    {id:3, title:'javascript', body:'javascript is ...'}
  ]
  **let content = null;
  if(mode === 'WELCOME'){
    content = <Article title="WELCOME" body="Hello.WEB"></Article>
  }else if(mode === "READ"){
    content = <Article title="READ" body="Hello.READ"></Article>
  }**
  return (
    <div>
        <Header title="WEB" onChangeMode={()=>{
            **setMode** ('WELCOME');
        }}></Header>
        <Nav topics={topics} onChangeMode={(id)=>{
            **setMode** ('READ');
        }}></Nav>
        {content}
    </div>
  );
  }

export default App;
```

- 클릭을 하면   setMode로  인해서 그 값이 READ(또는 WELCOME)로 바꾸고, App()컴포넌트가 다시 실행된다. 그러면서 useState가 mode의 값을 READ(또는 WELCOME)로 세팅해준다. IF문을 통해서 mode의 값이 READ였을 때 content가 실행되고 그것이 {content}로 화면에 실행되어 나타난다.
- mode와 setMode의 네임을 다르게 설정 가능하며, 그 네임이 위치해야 할 자리에 위치해 있으면 된다.

cnotent를 결정하는 title과 body의 글을 호출하기 위해 그것을 state로 만들어야 한다. 

- 숫자를 태그의 속성으로 넘기면 문자가 된다. 
문자를 숫자로 컨버팅해주는 넘버 함수 사용

```jsx
function App() {
  const [mode, setMode] = useState('WELCOME'); //지역변수를 state상태로 업그레이드
  const [id, setId] =useState(null); //현재 값이 없으니 초기값을 null로 지정한다. 
  const topics = [
    {id:1, title:'html', body:'html is ...'},
    {id:2, title:'css', body:'css is ...'},
    {id:3, title:'javascript', body:'javascript is ...'}
  ]
  let content = null;
  if(mode === 'WELCOME'){
    content = <Article title="WELCOME" body="Hello.WEB"></Article>
  }else if(mode === "READ"){
    let title, body = null; // title,body 초기화 
    for(let i=0; i<topics.length; i++){
      console.log(topics[i].id, id);
      if(topics[i].id === id){
        title = topics[i].title;
        body = topics[i].body;
      }
    }
    content = <Article title={title} body={body}></Article>
  }
  return (
    <div>
        <Header title="WEB" onChangeMode={()=>{
            setMode ('WELCOME');
        }}></Header>
        <Nav topics={topics} onChangeMode={(_id)=>{
            setMode ('READ');
            setId(_id); // nav에 있는 글을 클릭할 때 id값이 바뀌면 컴포넌트가 새로 실행되면서 새로운 id 값이 지정된다. 
        }}></Nav>
        {content}
    </div>
  );
  }

export default App;
```