## Eruda 专为前端**移动端**、**移动端**设计的调试面板



类似`Chrome DevTools` 的迷你版（没有chrome强大 这个是可以肯定的），其主要功能包括：捕获 `console` 日志、检查元素状态、显示性能指标、捕获XHR请求、显示`本地存储`和 `Cookie`信息、浏览器特性检测等等。





```shell
方式一，默认引入：
<script src="//cdn.jsdelivr.net/npm/eruda"></script>
<script>eruda.init();</script>

方式二，动态加载：

__DEBUG__ && loadJS('http://cdn.jsdelivr.net/eruda/1.0.5/eruda.min.js', ()=>{
  eruda.init();
});//苏南的专栏 交流：912594095、公众号：honeyBadger8

方式三 ，指定场景加载：
//比如线上 给自己留一个后门，
//我们一般的做法是喜欢给某个不起眼的元素，添加一个点击事件，要点它十次、八次以后才开启 debug 模式；
;(function () {
    var src = 'http://cdn.jsdelivr.net/eruda/1.0.5/eruda.min.js';
    if (!/eruda=true/.test(window.location) && localStorage.getItem('active-eruda') != 'true') return;
    document.write('<scr' + 'ipt src="' + src + '"></scr' + 'ipt>');
    document.write('<scr' + 'ipt>eruda.init();</scr' + 'ipt>');
})();

方式四 ，npm：
 npm install eruda --save
```



传送门 [https://github.com/liriliri/eruda](https://github.com/liriliri/eruda)