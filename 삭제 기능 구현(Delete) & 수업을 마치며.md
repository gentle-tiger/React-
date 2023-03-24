# 삭제 기능 구현(Delete) & 수업을 마치며

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
function Update(props){
  const [title,setTitle] = useState(props.title);
  const [body,setbody] = useState(props.body);
  return <article>
    <h2>Update</h2>
    <form onSubmit={event=>{
    event.preventDefault(); // 리로드를 멈춘다. 
    const title = event.target.title.value;
    const body = event.target.body.value;
    props.onUpdate(title,body);
  }}>
    <p><input type="text" name="title" placeholder="title..." value={title} onChange={event=>{
      console.log(event.target.value);
      setTitle(event.target.value);
    }}/></p>
    <p><textarea name="body" placeholder='body...' value={body} onChange={event=>{
      setbody(event.target.value);
    }}></textarea></p>
    <p><input type="submit" value="Update"></input></p>
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
  let contextControl = null;
  if(mode === 'WELCOME'){
    content = <Article title="WELCOME" body="Hello.WEB"></Article>
  }else if(mode === "READ"){
    let title, body = null; // title,body 초기화 
    for(let i=0; i<topics.length; i++){
      if(topics[i].id === id){
        title = topics[i].title;
        body = topics[i].body;
      }
    }
    content = <Article title={title} body={body}></Article>
    contextControl = <>
    <li><a href={'/update'+ id} onClick={event=>{
        event.preventDefault();
        setMode('UPDATE');
      }}>update</a></li>
    <li><input type="button" value="Delete" onClick={()=>{
        const newTopics = []  //오리지널과는 다른 데이터이다. 
        for(let i=0; i<topics.length; i++){
          if(topics[i].id !== id){
            newTopics.push(topics[i]);
          }
        }
        setTopics(newTopics);
      }}></input></li></>
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
  } else if(mode ==='UPDATE'){
    let title, body = null; // title,body 초기화 
    for(let i=0; i<topics.length; i++){
      if(topics[i].id === id){
        title = topics[i].title;
        body = topics[i].body;
      }
    }
    content = <Update title={title} body={body} onUpdate={(title,body)=>{
     console.log(title,body);
     const newTopics = [...topics]
     const updateTopic = {id:id, title:title, body:body}
      for(let i=0; i<newTopics.length; i++){
        if(newTopics[i].id === id){
          newTopics[i] = updateTopic;
          break;
        }
      }
      setTopics(newTopics);
      setMode('READ');
    }}></Update>
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
        <ul>
        <li><a herf="/create" onClick={event=>{
          event.preventDefault(); // 기존의 동작들을 실행하지 못하게 한다. 
          setMode('CREATE');
        }}>Create</a></li>
        {contextControl}
        </ul>
    </div>
  );
  }

export default App;
```