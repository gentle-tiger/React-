# 생성 기능 구현(Create)

- Create 기능은 기존의 list를 추가하는 것이다.
    - list를 작성한 text가 필요하기에 form을 통해서 그 틀을 만들어 준다. 그 값을 추가하였을 때 어디에 위치할 것인지 설정(기존의 값이 있을 경우 그 이후에 나타나도록 해야한다.) 해야 하며, 웹 페이지가 리로드가 되지 않도록 해야한다.

Created는 복잡하기 때문에 따로 변수로 뺴주도록 한다. 

state는 컴포넌트를 만드는 **내부자를 위한 데이터**이며, state를 사용하기 위해서는 react에서 usestate를 가져와야 한다. 

```jsx
import { useState } from 'react';
```

- 변수 선언

```jsx
const [state, setState] = useState(초기값);
```

```jsx
const state= state[0]; /* 라는 것은 상태의 값을 읽을 수 있다는 것을 의미한다. */
const setState= setState[1]; /* 1번째 mode 인 setMode를 통해서 mode의 값을 바꿀 수 있다는 규칙이 있는 것이다. */
```

state를 통해서 원하는 값을 읽은 후에 setState를 통해서 그 값을 변경해주는 것이다. 

```jsx
import logo from './logo.svg'; 
import './App.css';
import {useState} from 'react'

function Header (props){
  return <header>  
  <h1><a href="/" onClick={ event =>{
    event.preventDefault();  // preventDefault를 통해서 event를 제어한다. 클릭시 리로드 되지 않는다. 
    props.onChangeMode();
  }}>{props.title}</a></h1>
  </header>
}
function Nav (props) {
  const lis = []
  for(let i=0; i<props.topics.length; i++){
    let t = props.topics[i];
    lis.push(<li key={t.id}><a id={t.id} href={'/read/'+t.id} onClick={ event =>{
      event.preventDefault();
      props.onChangeMode(Number(event.target.id));
    }}>{t.title}</a>
    </li>)
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
function Create(props){
  return <article>
    <h2>Create</h2>
    <form onSubmit={event=>{
      event.preventDefault(); // 리로드를 멈춘다. 
      const title = event.target.title.value;
      const body = event.target.body.value;
      props.onCreate(title,body);
    }}>
      <p><input type="text" name="title" placeholder="값을 적어주세요."/></p>
      <p><textarea name="body" placeholder='본문을 입력해주세요. '></textarea></p>
      <p><input type="submit" value="Create"></input></p>
    </form>
  </article>
}
function App() {
  const [mode, setMode] = useState('WELCOME'); //지역변수를 state상태로 업그레이드
  const [id, setId] =useState(null); //현재 값이 없으니 초기값을 null로 지정한다. 
  const [nextId, setNextId] = useState(4);
  const [topics,setTopics] = useState([
    {id:1, title:'html', body:'html is ...'},
    {id:2, title:'css', body:'css is ...'},
    {id:3, title:'javascript', body:'javascript is ...'}
  ])
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
  } else if (mode === "CREATE"){
    content = <Create onCreate={(_title,_body)=>{
      const newTopic = {id:nextId,title:_title,body:_body}
      const newTopics = [...topics]
      newTopics.push(newTopic);
      setTopics(newTopics);
      setMode('READ');
      setId(nextId);
      setNextId(nextId+1);
    }}></Create>
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
        <a herf="/create" onClick={event=>{
          event.preventDefault(); // 기존의 동작들을 실행하지 못하게 한다. 
          setMode('CREATE');
        }}>Create</a>
    </div>
  );
  }

export default App;
```