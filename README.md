# Flutter_memos
Мои заметки по Flutter

Библиотека Flutter компонентов<br>
https://pub.flutter-io.cn/

Как сделать Splash экран загрузки для Flutter приожения?<br>
https://www.developerlibs.com/2018/07/flutter-how-to-fix-white-screen-on-app.html

Лучший WebView для Flutter<br>
https://pub.flutter-io.cn/packages/flutter_inappwebview

Документация по Bloc<br>
https://bloclibrary.dev

PUSH уведомления для Android и iOs версий приложений
https://medium.com/flutterdevs/push-notification-in-flutter-firebase-127289de5dd4

<h1>Заметки по архитектуре </h1>
<h3>Зависание приложении при переключении активностей работающих с Flutter движком</h3>
Ситуация следующая, старое Android приложение которое решили усовершенствовать используя Flutter SDK. Для совместимости с ранее реализованными механизмами пришлось делать несколько активностей и в каждой отображать то что отрисовывает Flutter Engine. По сути используется один движок, но он отображается на разных активностях в разное время. Как оказалось при переходе с одной активности Android на другую, которые используют экраны Flutter, движок уходит в небольшую паузу. Если быстро перейти с одной активности на другую произойдёт зависание, которое уже ничем не вылечить, кроме как свернуть приложение и открыть снова, или перейти к списку приложений и обратно. В частности у нас на одной активности происходит вход в систему, а на другой работа уже с контентом, и тот и другой экран отображают один Flutter Engine.
По всей видимости, когда мы переходим на второй экран, старый экран закрывается, и движку приходит сообщение что можно больше не работать, что он и делает, но уже на втором экране. Так как это происходит не моментально, на втором экране движок что-то успевает отрисовать и всё, пауза.
Чтобы это избежать, на втором экране работа с Flutter начинается не сразу а с паузой, в 500мс. За это время Flutter Engine успевает уйти в спячку, а затем мы её будим просто начав работать с ним.
По всей видимости Flutter Engine получает сообщение что работа прекратилась на одной активности, и при этом не отслеживат своё состояние на других, иначе бы он понял что его закрыли там, а тут с ним продолжают работать.
