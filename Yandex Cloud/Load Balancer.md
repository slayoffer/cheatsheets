# Yandex Network Load Balancer

Чтобы настроить сетевую балансировку в Yandex Network Load Balancer, разберёмся с двумя базовыми понятиями.

Первое — это **целевая группа**, т. е. набор серверов или других облачных ресурсов, по которым распределяются запросы пользователей. Целевая группа выглядит как список внутренних IP-адресов и подсетей, к которым эти IP-адреса относятся.

Допустим, вам нужно распределить трафик по пяти виртуальным машинам (ВМ). В этом случае целевая группа балансировщика может выглядеть так:

```
10.10.10.15, e9b7a3k9rqq3j0j36m9u
10.10.10.20, e9b7a3k9rqq3j0j36m9u
10.10.20.31, e2lgvksek5io187a48q5
10.10.20.10, e2lgvksek5io187a48q5
10.10.30.20, b0cnsvg8jfoe938ktqp4 
```

Здесь перечислены пять внутренних IP-адресов, причём для каждого адреса указан идентификатор его подсети. Все адреса целевых ресурсов должны принадлежать одной облачной сети.

Чтобы посмотреть список подсетей и их идентификаторов, откройте в консоли управления раздел **Virtual Private Cloud** и перейдите на вкладку **Облачные сети**.

Второе базовое понятие — **обработчик**. Это приложение принимает соединения от пользователей, распределяет их между IP-адресами целевой группы, а затем передаёт обратный трафик клиентам.

Адресация трафика строится по принципу 5-tuple: учитывается адрес и порт отправителя, адрес и порт целевого (принимающего) облачного ресурса, а также протокол передачи информации. Для приёма трафика обработчик использует порты от `1` до `32767`.

При создании сетевого балансировщика необязательно сразу настраивать обработчик. Если хотите, добавьте его позднее.

Кроме того, вы можете задать несколько обработчиков. Это пригодится, если запущенный на ВМ сервис предполагает использование нескольких портов сразу. К примеру, вы используете надстройку над Git наподобие GitLab. Значит, одновременно должны быть доступны и веб-интерфейс, и сервер Git, работающие на разных портах.

Целевую группу можно подключить к нескольким балансировщикам — например, чтобы балансировщики на портах `80` и `443` смогли обрабатывать и HTTP-, и HTTPS-запросы. Однако в этом случае вам придётся использовать разные целевые порты. Если группа подключена к одному балансировщику на порту `8080`, то к другому балансировщику вам придётся подключить её на порту `8081`.

После подключения целевой группы балансировщик начнёт проверять состояние целевых ресурсов и сможет распределять нагрузку между ними.

## Итак, у вас есть виртуальные машины. Можно сразу создать и балансировщик, и целевую группу, но мы поступим иначе: сначала создадим целевую группу, затем подключим её к балансировщику.

В консоли управления откройте раздел **Network Load Balancer,** на вкладке **Целевые группы** нажмите кнопку **Создать целевую группу**. На открывшейся странице введите имя целевой группы (например `demo-web`), выберите обе ВМ, созданные на предыдущем уроке, и нажмите кнопку **Создать**.

Остаётся создать балансировщик. Для этого сначала создайте обработчик и настройте проверку состояния ресурсов в целевой группе:

1. На вкладке **Балансировщики** нажмите кнопку **Создать сетевой балансировщик**.

2. Заполните имя балансировщика (например `lb-demo-web`) и нажмите кнопку **Добавить обработчик**.

3. В открывшемся окне введите имя обработчика (например 

   ```
   demo-web-listener
   ```

   ). В качестве портов укажите 

   ```
   80
   ```

   и нажмите кнопку 

   Добавить

   После создания обработчика нажмите кнопку 

   Добавить целевую группу

   . Укажите имя проверки состояния (например 

   ```
   hc-demo-web
   ```

   ), тип проверки (

   ```
   HTTP
   ```

   ), порт (

   ```
   80
   ```

   ), интервал отправки проверок состояния в секундах, порог работоспособности и порог неработоспособности. Оставьте указанный по умолчанию путь для проверок, используйте значения по умолчанию и для других параметров. Нажмите кнопку 

   Применить

   , а затем кнопку 

   Создать

4. После создания балансировщика проверьте состояние ресурсов: в консоли управления откройте страницу балансировщика и убедитесь, что его статус — 

   Active

   . Значит, балансировщик готов передавать трафик целевым ресурсам.

   Перейдите на страницу балансировщика и посмотрите на блок 

   Целевые группы

   . У запущенных ВМ, готовых принимать трафик, будет статус 

   Healthy

   .

   Введите внешний IP-адрес балансировщика в адресную строку браузера — и балансировщик перенаправит вас на одну из машин целевой группы. Обратите внимание на имя ВМ, указанное во второй строке веб-страницы.

   Чтобы протестировать отказоустойчивость, в консоли управления перейдите в раздел **Compute Cloud** и остановите одну из ВМ целевой группы.

   Вернитесь на страницу балансировщика и убедитесь, что статус остановленной ВМ изменился на **Unhealthy**. Это означает, что целевой ресурс группы не прошёл проверку состояния и не готов принимать трафик.

   

5. Обновите страницу с IP-адресом балансировщика, и вы увидите, что трафик перенаправлен на другую ВМ (изменилось имя ВМ, указанное во второй строке веб-страницы). 

После завершения работы не забудьте удалить использованные ресурсы: две ВМ и балансировщик.

# Как правильно использовать балансировщики

Чтобы построить эффективную инфраструктуру с высокой отказоустойчивостью:

- **Создавайте ресурсы в разных зонах доступности** 
  Размещайте копии виртуальных машин (ВМ) в нескольких зонах доступности. Так приложения останутся доступны, даже если одна из зон выйдет из строя. 
  В каждой зоне доступности разместите одинаковое количество облачных ресурсов. Если в `ru-central1-a` находится три ВМ — поместите по три ВМ и в `ru-central1-b`, и в `ru-central1-c`.
- **Создавайте облачные ресурсы с запасом** 
  Если одна ВМ в зоне доступности выйдет из строя, трафик продолжит поступать в зону в том же объёме, а нагрузка на оставшиеся машины увеличится. Чтобы все ВМ не вышли из строя — помимо ресурсов, необходимых для обслуживания расчётной нагрузки, добавьте в каждой зоне дополнительные вычислительные ресурсы (vCPU, RAM).
- **Используйте разные балансировщики для разных приложений** 
  Если вы разворачиваете в Yandex Compute Cloud несколько приложений — настройте для их обслуживания отдельные балансировщики. Это поможет эффективнее управлять нагрузкой.
- **Организуйте два уровня балансировщиков** 
  Балансировщики Yandex Cloud работают с протоколами TCP и UDP — так называемыми транспортными протоколами или протоколами четвёртого уровня [сетевой модели OSI](https://ru.wikipedia.org/wiki/Сетевая_модель_OSI). Они называются так потому, что предназначены для обеспечения надёжной передачи данных от отправителя к получателю. Кроме того, бывают балансировщики протоколов седьмого уровня — на этом уровне работает, например, протокол HTTP.

Поскольку балансировщики седьмого уровня, например веб-сервер NGINX, выполняют более сложную работу с IP-пакетами (сборка, анализ, журналирование), они выиграют от предварительного распределения нагрузки на четвертом уровне, особенно при DDoS-атаках.

Организуйте двухуровневую архитектуру с балансировщиками на четвертом (транспортном) уровне OSI и седьмом уровне приложения (например HTTP). Балансировщик четвертого уровня будет принимать трафик и передавать его целевой группе балансировщиков седьмого уровня, а те распределят трафик по ВМ с приложениями. В качестве балансировщиков седьмого уровня вы можете использовать ВМ, на которые установили ПО для балансировки (например NGINX).