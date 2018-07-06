认识有限，不对的地方一起探索  ：）

热更新框架的实现方式：
```
1：webview/hybird 
2：json协议 + 本地引擎 
3：C/C++  +   Javascript runtime + 本地 js  
4：framework  +   Javascript runtime  +  jsbundle
5：(C/C++ & 系统canvas）  +  (Dart runtime + 渲染器）（app内部）
```
热更新案例：
```
1：cocos2dx-lua   
2：JSPatch--Wax(OC-RUNTIME)   & android（ClassLoader）
3：Weex&RN  
4：Flutter
```     
优点：
```
1：跨平台
2：语法支持最火前端框架Vue
3：框架设计规范
4：hot - reload
5：集成、调试打包工具方便
6：组件可扩展Moudle，指定是否UI线程执行（RN 在 native_modules线程执行）
```
缺点：
```
1：学习成本（原生+web | RN基础）
2：性能 上  虽然是原生控件，但布局层次深
3：从vue的角度来说，没有dom，css阉割版，线性约束算法的 flex 布局
4：andorid包依赖过大(v8，ios使用本地的JsCore，RN 安卓 ios 都是 JsCore)
5：局限于框架，灵魂广度没有 RN 深远，限制开发者思维
```
