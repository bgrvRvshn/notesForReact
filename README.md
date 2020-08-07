# reactAbstract
> Ключевые особенности React: декларативность, универсальность, компонентный подход, виртуальный DOM, JSX.
## Компонент
- это **функция**, возвращающая разметку `JSX` и принимающая параметры `props`:
```jsx
const App = () => {
  return (
    <div>
      какая-то разметка
    </div>
  )
}
```  
- самостоятельно никогда не вызывается, вставляется с помощью тега:
```jsx
<App />
```
> Компонент всегда должен называться с большой буквы.

## import/export
При импорте javascript файлов, расширение писать не обязательно:
```jsx
import App from './App';
```
  При импорте других файлов, писать обязательно:
```jsx
import './App.css';
```
> Для того чтобы что-то импортировать, это должно быть экспортировано.  

По дефолту экспортируется только одна функция:
```jsx
export default App;
```
> При импорте модулей, не указывается путь, а только лишь название этого модуля в кавычках, например:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
```
  Название импортуемой фунции из файла по сути является переменной и может иметь произвольное наименование, которое может не совпадать с названием экспортируемой функции **по дефолту** в файле импорта. Например:
```jsx
import Abrakadabra from "./Technologies";

{/* СОДЕРЖИМОЕ ФАЙЛА TECHNOLOGIES.JS */}

const Technologies = () => {
    return <div>Какой-то текст</div>;
};

export default Technologies;
```
  Переменной `Abrakadabra` будет присвоено содержимое экспортируемое из файла `Technologies.js`. Также функция возвращает только один корневой `<div>`, исключение составляет использование модуля `<ReactFragment>` в которое оборачивается все содержимое `return()`.
  В одном компоненте могут одновременно экспортироваться несколько функций. При этом **по дефолту** экспортироваться может только одна. Остальные эскпортируются без ключевого слова `default`.
```jsx
const Foo = () => {
    return <div>Какой-то текст</div>;
};

const Bar = () => {
    return <div>Какой-то текст</div>;
};

export Foo;
export default Bar;
```
  В компоненте где такой компонент импортируется, следует использовать фигурные скобки `{}`, для функции которая не экспортируется по дефолту. Также таких функций может быть несколько, тогда стоит их перечилять через запятую в рамках одних фигурных скобок `import { BrowserRouter, Switch, Route } from "react-router-dom"`. Из одного компонента могут одновременно импортироваться фукнции как по дефолту так и нет. Пример: 
```jsx
import React, { Fragment, useContext } from "react";
```
  В данном примере компонент `React` экспортируется из `react` по дефолту, поэтому он вне фигурных скобок и может иметь произвольное наименование. А вот компоненты `Fragment, useContext` по дефолту не экспортируются, поэтому заключены в фигурные скобки, перечислены через запятую, а названия должны совпадать с названиями экспортируемых функций.

## Стили
  Импорт стилей производится по тому же правилу что и импорт компонентов, за исключением того, что из файла стилей экспортировать ничего не нужно.
```jsx
import "./App.css";
```
  В реакте чаще всего используются модули стилей. Для их использования необходимо в расширение файлов добавить префикс `module`. Например: `App.module.css`. При отсутсвии стилей использующих классы, а также если они не используются в компоненте где эти стили импортируются, импорт будет таким же, как и обычных стилей.
```jsx
import "./App.module.css";
```
#### Использование классов
  Классы в таких стилях имеют разные области видимости и никак не пересекаются, а на странице сайта они имеют префиксы, например класс `QuizList` на странице будет иметь вид `QuizList_QuizList__FdDtE`. Для использования классов в модульных стилях, необходимо задавать переменную наподобие той что мы задаем при импорте компонентов. Обзепринятым правилом является использование слова `classes`.
```jsx
import classes from "./App.module.css";
```
На странице компонента классы теперь необходимо задавать в фигурных скобках `{}`, а не в кавычках `""` как раньше. Классы задаются на странице компонента, элементам возвращаемой разметки, а не на странице вызова этого компонента. Например:
```jsx
render() {
  return (
    <div className={classes.Quiz}>
      Какое-то содержимое...
    </div>
  );
}
```
  Для использования препроцессоров, например `Sass`, необходимо установить `npm` пакет: `npm install node-sass`, а в расширения css файлов добавить расширение используемого препроцессора, например: `./App.module.scss`.
  > Не стоит забывать что `Class` в javascript является зарезервированным словом и не может использоваться для задания классов элементам, вместо него стоит использовать слово `className`.

## Props
Каждая компонента вызывается с параметрами (props). Пропс это объект параметров.
```jsx
const Header = (props) => {
  return (
    <div>
    Какое-то содержимое...
    </div>
  )
}
```
> Существует общепринятое соглашение о именовании параметров **пропсами**.  

  Реакт всегда вставляет какой-то объект в качестве пропса, в том числе пустой.
```jsx
let obj = {
  name: 'Dima'
}

Header(obj);
```
  Когда используем тег (компонент), внутрь тега по умолчанию попадает пустой пропс (объект с параметрами).
```jsx
<Header />

{
// empty object
}
```
  Наш компонент тоже является тегом, а значит мы тоже можем его настраивать извне с помощью атрибутов. Реакт автоматически создаст объект с ключом равным имени атрибута и значением равным значению атрибута. И к нам в тег попадет уже не пустой пропс, а со значением. Количество атрибутов переданных в компонент равняется количеству объектов в `props`.
```jsx
<Header name='Dima K' age='30' />

{
  name: 'Name K',
  age: '30'
}
```
  И наш тег получает доступ к пропсам. Посредством синтаксиса `props.name`.
```jsx
const Header = (props) => {
  console.log(props);

  return (
    <span> {props.name}, {props.age} </span>
  )
}
```
  Для обращения к объекту javascript внутри jsx (внутри конструкции `return()`), необходимо использовать фигурные скобки `{}`. Если же писать javascript до `return()`, то фигурные скобки писать не нужно, например: `console.log(props)`.  

## Роутинг
```bash
npm install react-router-dom
```
```jsx
import { BrowserRouter, Route } from 'react-router-dom'
```
  Некоторая система которая следит за адресом, реагирует на его изменения и загружать нужный компонент. Вместо обычного компонента будет использоваться компонент `<Route />`. Обязательными атрибутами являются `component`, где в качестве значения которого указывается компонент страницы, и `path` в котором указывается путь до компонента. Для использования роутинга необходимо весь возвращаемый `JSX` обернуть в `<BrowserRouter />`.
```jsx
const App = (props) => {
  return (
    <BrowserRouter>
      <div className='app-wrapper'>
        <Header />
        <Navbar />
        <div className='app-wrapper-content'>
          <Route path='/dialogs' component={ Dialogs } />
          <Route path='/profile' component={ Profile } />
        </div>
      </div>
    </BrowserRouter>
  )
}
```
  В случае передачи названия функции компонента в качестве значения атрибута `component`, передать `props` не представляется возможным. Вместо этого в качестве значения стоит передавать не название **функции компонента** `Dialogs`, а **анонимную стрелочную функцию** `() => {}`, с вызовом **тега компонента** `<Dialogs />`.
```jsx
<Route path='/dialogs' component={ () => <Dialogs /> } />
```
  Теперь в компонент можно передавать атрибуты (параметры (props)). Взамен атрибута `component` предпочтительнее использовать `render`.
```jsx
<Route path='/dialogs' component={ () => <Dialogs /> } />
```
Aтрибуты `component` и `render` нельзя использовать одновременно.
  Также ограничение при передаче названия функции компонента можно было обойти объявив **именованную стрелочную функцию**, до начала текущего тега, в который передать вызов тега нужного компонента. Например:
```jsx
let SomeComponent = () => <Dialogs />;

const App = (props) => {
  return (
    <div className='app-wrapper'>
        <Route path='/dialogs' component={ SomeComponent } />
    </div>
  )
}
```
#### NavLink
```jsx
import { NavLink } from 'react-router-dom'
```
  Основная задача - поменять `url` в браузере. Является компонентом модуля `react-router-dom`. Нужен для навигации по страницам сайта, используется в связке с `<Route />`. Обязательным атрибутом является `to`, которому присваивается значение `url` страница.  

  Компонент `<Route />` также понимает вложенные `url`, например при урле `/dialogs/spam/blabla` - также будет отображен компонент `/dialogs`. Для отключения такого поведения, используется атрибут `exact`, например `<NavLink to='/dialogs' exact>Profile</NavLink>`. В таком случае данный роут будет отображать компонент только при строгом соответствии пути.
```jsx
const Nav = () => {
  return (
     <ul>
      <li>
        <NavLink to='/profile'>Profile</NavLink>
      </li>
      <li>
        <NavLink to='/dialogs'>Dialogs</NavLink>
      </li>
     </ul>
  )
}
```
  Порядок работы Роутинга следующий:
1. Мы кликаем на компонент `<NavLink />`, на странице сайта она является обычной ссылкой, которая обрабатывается средствами javascript.  
   1. В адрес сайта добавляется `url` ссылки на которую мы кликнули.  
2. Компонент `<Route />` отлавливает изменения в адресной строке и сравнивает новое значение со значением указанным в атрибуте `path`.  
   1. Если оно совпадает, то возвращается содержимое компонента указанного в атрибуте `component`.  
   
  По умолчанию компонент `<NavLink />` активной ссылке добавляет класс `active`. Изменить это можно используя атрибут `activeClassName`.
```jsx
<NavLink to='/profile' activeClassName='activeLink'>Profile</NavLink>
```
## Работа с массивами
React в разметке `jsx` понимает написанный в нём массив, обязательно использовать фигурные скобки `{}`. Если прямо в `jsx` написать переменную содержащую массив из нескольких элементов (строка или реакт-компонент), то компилятор разложит его и отобразит каждый элемент отдельно.
```jsx
const Dialog = (props) => {
  {/* ДАННЫЕ (ДЛЯ ПРИМЕРА НАПИСАНЫ ПРЯМО В КОМПОНЕНТЕ) */}
  let dialogs = [   
    { id: 1, name: 'Dimych' },
    { id: 2, name: 'Slava' },
    { id: 3, name: 'Sasha' },
  ]
  
  {/* ИСПОЛЬЗУЕМ МЕТОД "MAP" ДЛЯ РАБОТЫ С МАССИВАМИ И ФОРМИРУЕМ НОВЫЙ МАССИВ */}
  let dialogsElements = dialogs
    .map( d => <DialogItem name={d.name} id={d.id} );
  
  {/* ВОЗВРАЩАЕМ РАЗМЕТКУ */}
  return (
    <div className="dialogs">
      <div className="dialogsItems">
        { dialogsElements } {/* ВСТАВЛЯЕМ МАССИВ С ЭЛЕМЕНТАМИ В ВИДЕ РЕАКТ КОМПОНЕНТОВ */}
      </div>
    </div>
  )
}
```

## UI - BLL (User Interface - React, Business Logic Layer - Redux)
  UI работает на основе полученных данных (props) и отрисовывает их. Данные в UI приходят не напрямую с сервера, а из прослойки, так называемой Логической (Business Logic). Layer - это слой отвечающий за хранение данных (набор файлов, функций, скриптов и т.д.). За хранение данных в Реакт отвечает Редакс. UI перерисовывается каждый раз когда изменяется состояние (state) в BLL. UI в свою очередь может делать запросы в BLL и изменять его.  
  В реакте реализован принцип Единой ответственности (Single Responsibility Principle (SRP)) - у каждого объекта есть своя ответственность и причина существования и эта ответственность у него только одна. Для этого и существует Редакс и разделение BLL и UI. Нарушение этого принципа является плохим архитектурным решением.  
  
## State
  Это простой объект, который хранит состояние (данные) компонента.  Если состояние изменяется, то компонент перерисовывается. Состояние можно изменять. Изменения могут основываться на ответе от пользователя, новых сообщениях с сервера, ответа сети и т.д. Состояние компонента должно быть приватным для компонента и контролироваться им. Изменения состояния компонента должны происходить внутри компонента - инициализация и обновление.  
  Компонент нуждается в `state`, когда данные в нём со временем изменяются. Например, компоненту `Checkbox` может понадобиться состояние `isChecked`, а компоненту `NewsFeed` необходимо отслеживать посты при помощи состояния `fetchedPosts`.  
  Самая больша разница между `state` и `props` состоит в том, что `props` передаются от родителя потомку, а `state` управляется самим компонентом. Компонент не может изменять `props`, но может изменять `state`.  
  Для каждой отдельной части изменяемых данных должен существовать только один компонент, который "управляет" изменением состояния. Не пытайтесь синхронизировать состояния двух разных компонентов. Вместо этого *поднимите оба этих состояния* до ближайшего компонента-родителя и передайте через пропсы необходим дочерним компонентам.
## onClick, Ref, VirtualDom
#### onClick
  `<button onClick={ () => { alert('Какое-то сообщение...') } }>Add post</button>`. Внутрь `onClick` передается колбэк-функция (функция обратного вызова - предназначенная для отложенного выполнения), в данном случае анонимная, стрелочная. Предпочтительнее такие функции описывать то `render ()`.
```jsx
const MyPost = (props) => {
  let addPost = () => {
    alert('Добавление поста');
  }
  
  return (
    <div className="postsBlock">
      <button onClick={ addPost }>Add post</button>
    </div>
  )
}
```
  При передаче функции в обработчик `onClick` скобки ставить не нужно, иначе функция вызовется немедленно, без события `click`.
#### Ref
  Использование в реакте методов `document.querySelector`, `document.getElementById` и т.д., для получения доступа к элементам на странице и взаимодействия с ними, не целесообразно (хоть и допускается), т.к. реакт на прямую не взаимодействует с DOM, а только лишь через VirtualDom. Для получения ссылки на элемент на странице используется метод `React.createRef()`, который предусматривает добавление атрибута `ref` со значением компоненту. Полный синтаксис использования вместе с `onClick` выглядит следующим образом:
```jsx
const MyPost = (props) => {
  let newPostElement = React.createRef(); { /* ПОЛУЧАЕМ ССЫЛКУ НА ЭЛЕМЕНТ */ }
  
  let addPost = () => {
    let text = newPostElement.current.value; { /* `current` - получение нативного html; `value` - получение значения */ }
    alert(text);
  }
  
  return (
    <div className="postsBlock">
      <textarea ref={ newPostElement }></textarea>
      <button onClick={ addPost }>Add post</button>
    </div>
  )
}
```
  Переменная `let newPostElement`, которой присваивается ссылка на элемент, является объектом DOM, со всеми свойствами.
## Callback через props
  Для того чтобы функцию находящуюся в родительском компоненте вызвать во вложенном компоненте с его параметрами, можно передать его через `props`. Для этого его нужно экспортировать в том компоненте где он описывается.
```jsx
let state = {
  profilePage: {
    posts: [
      { id: 1, message: 'Hi', likesCount: 12 },
      { id: 2, message: 'Hello', likesCount: 21 },
    ]
  }
}

export let addPost = (postMessage) => {
  let newPost = {
    id: 5,
    message: postMessage,
    likesCount: 0
  };
  
  state.profilePage.posts.push(newPost);
}

export default state;
```
## State management
  Вопрос о том как управлять `state`. 
  Один из подходов, это использование локального `state` классового компонента.  
  Есть два варианта с использованием локального `state`, один из них, это создание `state` в каждом из компонентов. Второй это создание `state` в каком-либо родительском файле, значения передавать через `props`, а изменять его с помощью функций. Но большой минус использования такого подхода, это появление большого количества функций для верного его изменения по мере увеличения приложения.  
  Для решения такой проблемы и были придуманы `redux и MobX`. `Redux` - функциональная парадигма программирования, а `MobX` - ООП.
## Инкапсуляция в React
```jsx
let man = {
  { /* 1 */ }
  name: "Dmitry",
  lastName: '',
  sayName() {
    alert(this.name);
  },
  { /* 2 */ }
  _content: '', { /* ПРИВАТНОЕ СВОЙСТВО */ }
  setContent(content) {
    this._content = this.content; // сеттер, срабатывает при записи
  },
  getContent() {
    return this._content; // геттер, срабатывает при чтении
  },
  render: function() {
    document.write(this._content);
  }
}
{ /* 1 */ }
console.log(man.age); { /* ОБРАЩЕНИЕ К СВОЙСТВУ ОБЪЕКТА */ }

man.lastName = 'Kovalchuk' { /* УСТАНОВКА ЗНАЧЕНИЯ СВОЙСТВУ */ }

man.sayName(); { /* ОБРАЩЕНИЕ К МЕТОДУ ОБЪЕКТА */ }

{ /* 2 */ }
man.setContent('Какое-то содержимое...'); { /* Установка нового значения */ }

console.log( man.getContent() ); { /* Получение значения */ }

man.render();
```
  Существует общепринятное правило, что если свойство или метод имеет знак нижнего подчеркивания `_` вначале, то такое свойство или метод являются приватными, а значит не стоит изменять его напрямую, например `man._content = 'Какое-то содержимое...'`. Для изменения таких свойств следует использовать [геттеры и сеттеры](https://learn.javascript.ru/property-accessors). Это свойства-аксессоры (accessor properties). По своей сути это функции, которые используются для присвоения и получения значения, но во внешнем коде они выглядят как обычные свойства объекта. Свойства-аксессоры представлены методами: «геттер» – для чтения и «сеттер» – для записи.
## Store
  Это концепция используемая для распределения состояния между компонентами. В основном это простой объект `JavaScript`, который позволяет компонентам совместно использовать состояние. В нем может храниться как и сам `state`, так и методы работы с ним.
## Dispatch & Action)
  `Dispatch` (пер. Отправка) - это метод объекта `store` для более удобной работы со всеми остальными методами. Он принимает в себя 2 обязательных параметра: `action` и `type` (дополнительных может быть больше), имеет вложенные конструкции `if else`. Используется для изменения состояния `store`.  
  `Action` (пер. Действие) - это стандартный аргумент `dispatch` - объект, у которого обязательно есть свойство `type`, благодаря нему мы можем обрабатывать различные действия по их типу, попадая в нужную секцию `if else`.
```jsx
  // state.js
  let store = {
    _state: '',
    dispatch(action) { // { type: 'ADD-POST' }
      if (action.type === 'ADD-POST') {
        let newPost = {
          id: 5,
          message: this._state.profilePage.newPostText,
        }
      } else if (action.type === 'UPDATE-NEW-POST-TEXT' ) {
        this._state.profilePage.newPostText = action.newText;
        this._callSubscriber(this._state);
      }
    }
  }
  
  // index.js
  <App state={ state } dispatch={ store.dispatch.bind(store) } />
  
  // App.js
  <Route path='/profile' render={ () => <Profile dispatch={ props.dispatch } /> } />
  
  // Profile.js  
  let onPostChange = () => {
    let text = newPostElement.current.value;
    
    props.dispatch({ type: 'UPDATE-NEW-POST-TEXT', text: text });
  }
  
  return (
    <textarea onChange={ onPostChange } ref={ newPostElement } ></textarea>
  )
```
## Action creator, Action type
  Action creator - это функция которая возвращает объект `action` для использования в `dispatch`.
  Action type - это строковая константа, которая записывается в свойство `type` объекта `action`.
```jsx
const ADD_POST = 'ADD-POST';
const UPDATE_NEW_POST_TEXT = 'UPDATE-NEW-POST-TEXT';

// state.js
let store = {
  _state: '',
  dispatch(action) { // { type: 'ADD-POST' }
    if (action.type === ADD_POST) {
      let newPost = {
        id: 5,
        message: this._state.profilePage.newPostText,
      }
    } else if (action.type === UPDATE_NEW_POST_TEXT ) {
      this._state.profilePage.newPostText = action.newText;
      this._callSubscriber(this._state);
    }
  }
}

export const addPostActionCreator = () => {
  return {
    type: ADD_POST
  }
}

export const updateNewPostTextActionCreator = (text) => {
  return {
    type: UPDATE_NEW_POST_TEXT,
    newText: text
  }
}

// Profile.js
import { addPostActionCreator, updateNewPostTextActionCreator } from './../...state.js';

let addPost = () => {
  props.dispatch( addPostActionCreator() );
}

let addPost = () => {
  let text = newPostElement.current.value;
  
  props.dispatch( updateNewPostTextActionCreator( text ) );
}
```
## Reducer
  Это чистая функция которая принимает `state` и `action`, если нужно применяет этот `action` к `state` и возвращает новый `state`, либо неизмененный `state`.
```jsx
reducer(state, action) {
  ...
  return changedState;
}
```
  После возвращения нового `state`, "подписчики" будут уведомлены о изменениях в `state` и смогут запросить изменения с помощью `getState`.
## Redux
```bash
npm install redux --save
```
  В `redux` по умолчанию есть функция по созданию `store`. Также `redux` по умолчанию уведомляет своих подписчиков об изменении `state`, но не передает им `state`. Для этого требуется с вызовом функции рендеринга DOM дерева передавать ей `state` в параметрах.
```jsx
// index.js
let rerenderEntireTree = (state) => {
  ReactDOM.render(
    <BrowserRouter>
      <App state={state} dispatch={store.dispatch.bind(store)} store={store} />
    </BrowserRouter>, document.getElementById('root'));
  )
}

rerenderEntireTree(store.getState()); // ПОЛУЧАЕМ STATE С ПОМОЩЬЮ ВСТРОЕННОГО МЕТОДА REDUX

// УВЕДОМЛЕНИЕ ПОДПИСЧИКОВ ПРИ ИЗМЕНИИ STATE
store.subscribe(() => {
  let state = store.getState();
  rerenderEntireTree(state);
})
```
  В переменную `reducers` записываются `state` в качестве названия свойства объекта и сами `reducers` в качестве значений.
```jsx
// redux.js
import { combineReducers, createStore } from "redux";
import profileReducer from "./profile-reducer";
import dialogsReducer from "./dialogs-reducer";
import sidebarReducer from "./sidebar-reducer";

let reducers = combineReducers({
  profilePage: profileRecuder,
  dialogsPage: dialogsReducer,
  sidebar: sidebarReducer
});

let store = createStore(reducers);

export default store;
```
## Container component
  Это **контейнерная компонента** которая является оберткой над комопнентом. Может не удовлетворять требованиям чистой функции. Она будет получать в себя `store`, а также проводить все манипуляции с ним, для дальнейшей передачи в **презентационную компоненту**.
```jsx
// App.js
<Route render = { () => <DialogsContainer store={ props.store } /> } />
////

// DialogsContainer.jsx
import Dialogs from './Dialogs.js';

const DialogsContainer = (props) => {
  let onNewMessageChange = (body) => {
    props.state.dispatch(updateNewMessageBodyCreator(body));
  }
  
  return <Dialogs updateNewMessageBody={ onNewMessageChange } />
}

export default DialogsContainer;
////

// Dialogs.js
let onNewMessageChange = (e) => {
  let body = e.target.value;
  props.updateNewMessageBody(body);
}
////
```
  То есть для `render` мы уже передаем компонент `DialogsContainer`, в него же передаем `props`, проводим все манипуляции с ним и возвращем компонент `Dialogs`.
## Контекст
  Контекст позволяет передавать данные через дерево компонентов без необходимости передавать пропсы на промежуточных уровнях. Контекст разработан для передачи данных, которые можно назвать "глобальными" для всего дерева Реакт-компонентов (например, текущий текущий аутентифицированный пользователь, UI-тема или выбранный язык).  
  Обычно контекст используется, если необходимо обеспечить доступ данных во *многих* компонентах на разных уровнях вложенности. По возможности не стоит его использовать, так как это усложняет повторное использованиекомпонентов.
> Если необходимо избавиться от передачи нескольких пропсов на множество уровней вниз, обычно **[композиция компонентов](https://ru.reactjs.org/docs/composition-vs-inheritance.html)** является более простым решением, чем контекст.  

  Для использования контекста его нужно создать с дефолтным значением `const MyContext = React.createContext(defaultValue);`. Далее копмонент должен создать `Provider` (поставщик) и передать в него значение `< MyContext.Provider value={ /* store value */ } />`. Дочерний компонент принимает контекст и яляется `Consumer` (потребителем) `<MyContext.Consumer> { value } </MyContext.Consumer>`.
```jsx
// DialogsContainer.js
const DialogsContainer = () => {
  return <StoreContext.Consumer>
  {
    (store) => {
      let onNewMessageChange = (body) => {
        store.dispatch(updateNewMessageBodyCreator(body));
      }

      return <Dialogs updateNewMessageBody={ onNewMessageChange } />
    }
  }
  </StoreContext.Consumer>
}
```
### Понятия и Термины
###### The Single Responsibility Principle, SRP
  Принцип единственно ответственности - это принцип ООП означающий, что каждый объект должен иметь одну ответсвенность и причину существования.
###### Чистая функция
- Каждый раз возвращает одинаковый результат, когда она вызывается с тем же набором аргументов.  
- Не имеет побочных эффектов. Побочные эффекты включают: меняющийся вход, HTTP-вызовы, запись на диск, вывод на экран.  
- Можно безопасно клонировать, а затем менять входные параметры, оставив оригинал без изменений.
###### Callback
  Колбэк-функция или функция обратного вызова предназначена для отложенного выполнения.
```jsx
let addUser = () => { // logic here }

<button onClick={ addUser }>Add user</button>
```
В данном примере функция была передана в качестве параметра в тег `button` и будет вызвана при событии `onClick` на кнопке. Фукнция передается туда без скобок `()`, то есть не вызывается сразу.
###### FLUX
  Это архитектурный подход или набор шаблонов программирования для организации однонаправленного потока данных в приложении. Одна из наиболее популярных реализаций flux-архитектуры - `Redux`.
###### Замыкание
  Фукнция в теле которой присутствуют ссылки на переменные, объявленные вне тела этой функции в окружающем коде и не являющиеся её параметрами. По сути это способность функции к поиску переменных в глобальной области видимости, если используемые в ней переменные не объявлены внутри тела функции и не передаются в качестве параметров.
###### Observer
  Это поведенческий паттерн проектирования, который создаёт механизм подписки, позволяющий одним объектам следить и реагировать на события, происходящие в других объектах.  
  Паттерн "Наблюдатель" - определяет отношение "один ко многим" между объектами таким образом, что при изменении состояния одного объекта (**субъекта**) все зависящие от него объекты(**наблюдатели**) получают уведомление об этом и также обновляют своё состояние. Это нужно для согласования состояния взаимосвязанных объектов без их жесткой связанности (циклической зависимости). Данный паттерн реализован при добавлении слушателя событий в js `addEventListener` или `onClick, onChange` и т.д.
###### Инкапсуляция
  Это объединение данных и функций, которые с этими данными работают, в рамках одной структуры (в объекте), внутреннее состояние которой скрыто от прямого внешнего обращения (*сокрытие деталей*). Функции в таких объектах называются методами.
###### This (Контекст)
  В javascript методами называют функции, записанные в свойства объектов. Фактически метод - это роль, которую выполняет функция будучи привязанной к объекту. В JS функции это объекты *первого рода*, то есть они ведут себя как данные: их можно записывать в переменные или константы. Свойства объектов подобны переменным, а значит в них можно сохранить функции.  
  Для доступа к данным объекта внутри метода используется ключевое слово `this`. Внутри методов оно ссылается на текущий объект, к которому привязан метод. Правда так `this` работает только для обычных функций. У стрелочных другой принцип.
###### Bind
  Метод привязывает контекст к функции. В качестве первого параметра следует передавать контекст, а последующими параметрами - параметры функции. Метод возвращает новую функцию, внутри которой `this` будет равным переданному контексту.
```js
функция.bind(контекст, параметр1, параметр2...);
```
  Не обязательно записывать результат работы `bind` в новую функцию, можно просто перезаписать.
