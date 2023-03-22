# props

<aside>
💡 React에서 함수의 이름의 첫 번쨰 문자는 대문자로 설정한다.

</aside>

**header에 title의 값으로 ract를 주고 그 값을 props를 통해 불러오고 싶다면?** 

- a 태그의 “/”는 해당 디렉토리를 의미한다.
- 함수의 첫 번째 인자값으로 props를 지정하여 props가 객체라는 것을 확인한다.
porps.title을 하게 되면 props가 가지고 있는 title의 값을 불러오게 하여 적용시킨다. 
이렇게 하여 react라는 텍스트를 얻으려면 props.title을 하면 된다.
- props.title의 값을 다른 곳에서도 불러오고 싶다면, 그 값을 {}중괄호 안에 넣어서 실행한다.

```jsx
import logo from './logo.svg'; 
import './App.css';

 function Header (props){
  return <header>  
  <h1><a href="/">{props.title}</a></h1>
  </header>
}
function App() {
  return (
    <div>
        <Header title="react"></Header>
        <Nav></Nav>
        <Article></Article>
    </div>
  );
  }

export default App;
```

```jsx
console.log('props :', props,'props.title :',props.title); 을 했을 때 

> props {title: 'react'}
				title: "react"
				[[Prototype]]: Object  라는 결과값이 나타나는 것이다. 

props : {title: 'react'} props.title : react

```

- props 에는 객체가 들어오는데, 그 객체는 title이 react다 라고 나온다.

**Article에 title의 값은 Welcome, body의 값은 hello.Web을 주고, 그 값을 가져오고 싶다면?**

```jsx
function Article (props){
  return  <article>
  <h2>{props.title}</h2>
  {props.body}
 </article>
}

function App() {
  return (
    <div>
        <Header title="react"></Header>
        <Nav></Nav>
        <Article title="Welcome" body="hello.web"></Article>
        <Article title="hi" body="거북이"></Article>

    </div>
  );
  }

export default App;
```

- Nav도 같은 방법을 적용해보자.

그러기 위해서 topics라는 변수를 만들어 줄 것이다. (topics 변수는 이 함수안에서는 바뀌지 않기 때문에 const를 적용해준다. topics라는 값을 뒤에서 바꿀 수 없게 한다.) 

```jsx
function App() {
  const topics = [
    {id:1, title:html, body:'html is ...'}
    {id:2, title:css, body:'css is ...'}
    {id:3, title:javascript, body:'javascript is ...'}

  ]
  return (
    <div>
        <Header title="WEB"></Header>
        <Nav topics="topics"></Nav>
        <Article title="Welcome" body="Hello.WEB"></Article>
    </div>
  );
  }

export default App;
```

- App 함수 안에 topics라는 변수를 넣고 그 변수를 Nav의 topics의 값으로 전달했다. 하지만 이 상태에서는 topics의 값은 문자열 그대로를 출력하기에 {topics}로 바꿔준다.

```jsx
<Nav topics={topics}></Nav>
// 문자열로 전달되는 것이 아닌 있는 그대로 전달된다. 
```

- topics의 배열만으로는 코드를 재활용하기 힘들다는 것이다.
- 이 코드를 for문을 통해서 그 값을 가져올 것이다.

최종 컴포넌트

```jsx
import logo from './logo.svg'; // I want to use babel-eslint for parsing!
import './App.css';
function Header (props){
  console.log('props', props, props.title);
  return <header>  
  <h1><a href="/">{props.title}</a></h1>
  </header>
}
function Nav (props) {
  const lis = []
  for(let i=0; i<props.topics.length; i++){
    let t = props.topics[i];
    lis.push(<li key={t.id}><a href={'/read/'+t.id}>{t.title}</a></li>)
  }
  return <nav>
  <ol>
    {lis}
  </ol>
</nav>
}
function Article (props){
  return  <article>
  <h2>{props.title}</h2>
  {props.body}
 </article>
}

function App() {
  const topics = [
    {id:1, title:'html', body:'html is ...'},
    {id:2, title:'css', body:'css is ...'},
    {id:3, title:'javascript', body:'javascript is ...'}
  ]
  return (
    <div>
        <Header title="WEB"></Header>
        <Nav topics={topics}></Nav>
        <Article title="Welcome" body="Hello.WEB"></Article>
    </div>
  );
  }

export default App;
```