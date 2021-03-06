###Фреймворк для работы с такой концепцией как Promise. Подробнее про него можно прочитать вот [тут](http://promisekit.org "PromiseKit").
####Promise - это способ организации работы с асинхронным кодом в виде цепочек действий.

1. Асинхронный код становится простым и линейным вне зависимости от того, в каком потоке он исполняется. Перед глазами его строгая структура — что и зачем выполнится. На одном экране можно поместить следующего вида логику: Загрузить два конфига. Когда они докачаются — проверить их на валидность. После этого запустить какую-то анимацию, после этого отобразить результаты.

2. Асинхронный код становится атомарным, инкапсулированным и реиспользуемым. Очень часто появляется много соблазнов разрушить инкапсуляцию асинхронных операций — использовать какие-то общие переменные, например — что делает асинхронный код более сложным для изменений.

3. Появляется комфортная возможность для обработки ошибок всей цепочки исполнения. Каждый кто писал цепочки асинхронного кода сталкивался с тем, что чем длиннее цепочка — тем сложнее корректно обработать ошибки, тем сложнее и более громоздким становится код. 

####Достоинства 
*	Значительно упрощает построение графа исполнения асинхронных операций. Просто сделать, чтобы одна операция началась сразу же за другой или же за несколькими другими
*	Значительно упрощает обработку ошибок, возникающих на разных этапах исполнения цепочки (особенно, в том случае, когда цепочка в приложении имеет смысл только в случае полного успеха всех операций и не имеет смысла во всех остальных)
*	Позволяя грамотно инкапсулировать асинхронные операции — значительно облегчает их множественное использование в разных местах приложения.


####Недостатки 
*	По прежнему необходимо быть аккуратным с памятью при работе с промисами
*	Нужно некоторое время на то, чтобы привыкнуть работать не с «кодом», а с «оберткой, которая вызовет твой код»
*	Неудобство во время дебага.



**Пример создания обертки Promise:**
```
- (PMKPromise *)users {
   return [PMKPromise new:^(PMKPromiseFulfiller fulfill, PMKPromiseRejecter reject) {
        PFQuery *query = [PFUser query];
       [query whereKey:@"name" equals:@"mxcl"];
        [query findObjectsInBackgroundWithBlock:^(NSArray *objects, NSError *error) {
            if (error) {
               reject(error);
            } else {
                fulfill(objects);
            }
        }];
    }];
}
```
Вызывает  futfill если асинхронная операция выполнена, и reject в случае ошибки.

**Использование:**
```
[self users].then(^(NSArray *users){
    //…
});
```

Взято [отсюда](http://habrahabr.ru/post/254435/ "Записки iOS программиста о его молотках, кувалдах и микрометрах")
