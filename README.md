# `react-course`
## React Lesson: 3 [ router, redux ]

`npm install`Устанавливаем все необходимые для работы пакеты из package.json (зависимости) <br/>
`npm run dev`Запускаем проект (запускается скрипт прописаный внутри package.json с ключем "dev")
<br/><br/>
**Router** (react-router) - модуль подключаемый к react который позволяет отрисовывать определенные компоненты в зависимости от пути 
(в адрессной строке, например `/about`)
<br/><br/>
Работа с этим модулем достаточно проста поскольку он сделан в виде компонентов.
<br/><br/>
Для того чтобы начать работу с react-router его разумеется для начала необходимо установить в нашу 
папку с помощью `npm i react-router-dom` (так как я уже указал это в `package.json` 
нужно написать лишь `npm i` и он доустановит не достающие пакеты). Нужно устанавливать именно `react-router-dom` так как в последней 
версии роутера все необходимое для работы с роутером в вебе собрано в модуле с таким названием. По той же причине по которой в реакт есть разделение на react - ядро библиотеки, и `react-dom` модуль библиотеки 
который отвечает непосредственно за работу с браузером.
<br/><br/>
На текущий момент последней версией `react-router` является `v4` которую мы собственно и используем. Стоит учесть, что различия 
в версиях этого модуля влечут за собой определенные изменения в структуре а также логике использования этого модуля. Об этом можно более детально 
прочесть в статьях посвещенных этому вопросе.
<br/><br/>
**BrowserRouter** -  это основной компонент react-router'a который позволяет нашему приложению работать с url с помощью 
History API который позволяет меняеть url без перезагрузки страницы.
[Детальнее](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/BrowserRouter.md)
<br/>
**Route** -  Route - компонент принимающий два свойства:
1. path - url
2. component - компонент который отобразиться по указаному в path url.
так-же присуствует возможность делать компонент парным, что дает возможность
вкладывать в него другие теги и компоненты. Нужен для того чтобы отоборажать определенные 
компоненты при определенных url.
[Детальнее](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/Route.md)
<br/>

**Switch** - вспомогательный компонент который позволяет групировать определенные
`Route` и переключаться между ними. 
[Детальнее](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/Switch.md)
<br/>
**Link** - необходим для того чтобы переключатся между "страницами", по факту - аналог 
обычного `<a>`, но работает с помощью BrowserHistory или hashHistory вместо привычного нам href нужно 
писать `to={"/some-url"}` 
[Детальнее](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/Link.md)
<br/><br/>
**Redux** это библиотека которая позволяет объединить все данные в одном месте для упрощения взаимодействия с ними.
Без redux вам приходилось использовать пробрасывание данных из компонента в компонент, на простых проектах разумеется 
это не такая большая проблемма, но в случае с большими проектами возникает проблема - все слишком запутано, эту проблему 
как-раз таки и решает redux. 
<br/>
В redux есть определенный принцип, начнем пожалуй с того что взаимодействие с всеми данными контролируется одним объектом 
именуемым - store. На самом деле, если посмотреть на этот объект с помощью console.log можно заметить что прямого доступа 
к данным нет, но есть метод getState() который нам их любезно предоставляет. Так-же в этом объекте есть и другие методы, но о них позже.
<br/><br/>
**xxx:** Окей, теперь мы знаем что есть какой-то store и из него с помощью определенного метода можем получить данные. и что?
<br/>
**yyy:** Теперь нужно понимать, как именно нам эти данные туда ложить, и как именно давать нашим компонентам знать о том что мы это сделали. 
<br/><br/>
Для того чтобы изменить наши данные нужно об этом как-то оповестить наш `store` о том что мы хотим изменить данные, в redux этим 
звеном выступают так называемые `actions`.
<br/>
**Actions** - в контексте нашего разговора это функция которая возвращает объект. Но не просто возвращать, а возвращать с помощью 
функции dispatch которая опять же находиться в нашем объекте `store`, но принято все это дело упрощать, 
вместо того чтобы каждый раз писать: return `dispatch({ type: t.ADD_POST, payload })` внутри нашего `action` мы делаем это 
уже при подключении нашего компонента в специальной функции mapDispatchToProps, но об этом чуть позже.
<br/>
Как показано в примере, это обычная функция, которая возвращает объект вида `{ type: types.ADD_POST, payload }`
<br/><br/>
_`type`_ - нужен для того чтобы обработчик нашего события (о котором вы тоже узнаете чуть чуть позже) понимал о том что конкретно 
ему нужно сделать
<br/>
_`payload`_ - это данные с которыми нашему обработчику что-то нужно сделать, их можно передавать, а можно не передавать, зависит 
от того что мы хотим сделать
<br/><br/>
Под каждое событие мы должны писать свой `action` который по своему влияет на данные.
Каждый `action` влечет за собой перересовку компонента который подписан на изменения, как это 
сделать вы узнаете чуть позже.
<br/>
Каждый такой `action` как я уже и сказал чем-то обрабатывается, это "что-то" называеться `reducer`, он занимается лишь 
обработкой наших обращений к `store` по средствам `actions`. 
По принятым правилам reducer должен быть чистой функцией. <br/>
_**Чистая функция** — это функция, которая при одинаковых аргументах всегда возвращает одни 
и те же значения и не имеет видимых побочных эффектов._

<br/><br/>

```
export default function posts(state = InitialState.posts, action) {
   let {type, payload} = action;

   switch(type) {
       case types.ADD_POST:
           return [...state, payload];
       default:
           return state;
   }
};
```

<br/><br/>

Это reducer, он обрабатывает наши actions. Можно создать несколько редьюсеров для разных
по смыслу событий.

Reducer это функция (всегда), которая принимает на вход два параметра,
**первый** - **state**, это кусочек данных из всего приложения который мы можем с помощью
этого редьюсера изменять.
**второй** - **action**, это событие которое мы вызвали.

Для того чтобы наш редьюсер отработал нам нужно добавить его в наш объект  `store`

