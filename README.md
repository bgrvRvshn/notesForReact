# reactAbstract
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
  Название импортуемой фунции из файла по сути является переменной и может иметь произвольное наименование, которое может не совпадать с названием экспортируемой функции в файле импорта. Например:
```jsx
import Abrakadabra from "./Technologies";

{/* СОДЕРЖИМОЕ ФАЙЛА TECHNOLOGIES.JS */}

const Technologies = () => {
    return <div>Какой-то текст</div>;
};

export default Technologies;
```
  Переменной `Abrakadabra` будет присвоено содержимое экспортируемое из файла `Technologies.js`. Также функция возвращает только один корневой `<div>`, исключение составляет использование модуля `<ReactFragment>` в которое оборачивается все содержимое `return()`.

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
 
