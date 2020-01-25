### Opentutorials React.js 과정 
+ 생활코딩 이고잉님의 강의 영상을 기반으로 구현한 프로젝트입니다 

(URL https://www.opentutorials.org/module/4058)

### 프로젝트 목표
+ react.js의 개발환경 구축 및 기초문법 숙달

### 개발환경 구축
+ npx install create-react-app
### 커리큘럼
+ 1일차(01.12)  
  + 컴포넌트 제작
  + 컴포넌트의 props 사용
  + 컴포넌트 분리 
+ 2일차(01.13)
  + state 구현 (App.js)
  ```react.js
    constructor(props){
    super(props);
    // App 컴포넌트가 내부적으로 사용할 상태는 여기서 지정한 state
    this.state={
      // subject라는 이름의 property (state상에서 존재)
      subject:{title:'WEB', sub:'world wide web!!'},
      contents:[
        {id:1, title:'HTML', desc:'HTML is ...'},
        {id:2, title:'CSS', desc:'CSS is ...'},
        {id:3, title:'JavaScript', desc:'JavaScript is ...'}
      ]
    }
  }
  ```
  > components 디렉토리의 각 컴포넌트로 전달하기 위한 state 지정 
  + state를 props로 각 컴포넌트에 전달
  ```react.js
  <Subject 
    // App(상위 컴포넌트)의 state를 가져옴
    title={this.state.subject.title} 
    sub={this.state.subject.sub}>
  </Subject>
  
  <TOC 
    // TOC 컴포넌트에 data라는 이름의 props전달
    data={this.state.contents}>
  </TOC>
  
  <Content title={_title} desc={_desc}>
  </Content>
  ```
  + state 수신(./components/Content.js)
  ```react.js 
  <h2>{this.props.title}</h2>
  ```
+ 3일차(01.14 ~ 01.15)
  + 앱의 이벤트 상황을 기억하기 위한 state 지정 (App.js)
  ```react.js
   mode:'read',
   welcome:{title:'Welcome', desc:'Hello, React!'}, 
  ```
  ```react.js
  // 웹 페이지의 status를 확인하고 그에 따른 동적 화면 출력
  var _title, _desc = null;
  if(this.state.mode === 'welcome') {
    _title = this.state.welcome.title;
    _desc = this.state.welcome.desc;
  } else if (this.state.mode === 'read') {
    _title = this.state.contents[0].title;
    _desc = this.state.contents[0].desc;
  }
  ```
  + event 구현
  ```react.js
    <header>
      {/*클릭 이벤트 설치(onClick)*/}
      <h1><a href="/" onClick={ function(e){
      // 페이지 리로드를 막기 위한 함수
        e.preventDefault();
      // setState : state의 값을 변경하고 컴포넌트에 알림
        this.setState({
          mode : 'welcome' 
        });
      // 익명 function 안에 사용된 this는 undefined를 가리키므로 
      // .bind(this)를 사용해서 해당 컴포넌트를 강제로 가리키게 함
      }.bind(this)}

      >{this.state.subject.title} </a>
      </h1>
      {this.state.subject.sub}
    </header>
    ```
  > 개념 이해를 위해 Subject.js의 컴포넌트를 App.js로 가져와서 구현
+ 4일차(01.16 ~ 01.19)
  + 현재 선택된 컨텐츠의 id를 지정하는 state 
  ```react.js
  selected_content_id:1,
  ```
  + 현재 순번 컨텐츠 id와 선택한 컨텐츠의 id값 비교
  ```react.js
  var i = 0;
  while(i<this.state.contents.length){
    var data = this.state.contents[i];
    if(data.id === this.state.selected_content_id) {
      _title = data.title;
      _desc = data.desc;
      break;
    }
    i = i+1;
  }
  ```
  + 이벤트 발생시 state를 변경하고, render 재호출
  ```react.js
  <TOC 
  onChangePage={function(id){
    this.setState(
      {
        mode:'read',
        selected_content_id:Number(id),
      }
      );
  }.bind(this)}
  data={this.state.contents}>
  </TOC>
  ```
  > 익명함수의 인자(클릭한 태그의 id값)를 setState()
  + 클릭 발생시 상위컴포넌트의 함수 실행(TOC.js)
  ```react.js
  data-id = {data[i].id}
  ```
  > 각 리스트의 a태그에 data-id라는 속성과 id값을 지정함
  ```react.js
  this.props.onChangePage(e.target.dataset.id);
  ```
  > 태그내 익명함수시 생기는 e객체로 해당태그로 접근
  > 위에서 지정한 data-id 속성은 dataset이라는 카테고리에서 확인가능
+ 5일차(01.20)
  + create 구현
  + components 디렉토리에 Control컴포넌트를 추가하고, Content컴포넌트를 CreateContent와 ReadContent로 분할
  + Control컴포넌트에 모드를 변경하기 위한 항목 추가
  ```react.js
  <Control onChangeMode={function(_mode){
    this.setState({
      mode : _mode,
    });
  }.bind(this)}>
  </Control>
  ```
  + 모드변경시 컴포넌트를 변경하기 위한 항목 추가
  ```react.js
  else if(this.state.mode ==='create') {
      _article = <CreateContent></CreateContent>
    }
  ```
  > _article 항목에 태그를 저장, read모드일 경우에는 ReadContent컴포넌트와 title, desc값 전달
  + CreateContent 컴포넌트에 입력하기 위한 폼 태그 작성
+ 6일차(01.21 ~ 01.22)
  + update & delete 구현
