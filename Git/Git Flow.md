**GIT FLOW по “взрослому”**

**main - основная ветка, в ней должен быть только рабочий кот).**

**основное правило - ничего не пушить напрямую в main!**

 

\1.   Находясь в основной ветке создаем ветку по названию **задачи** - 
 **git checkout -b "название_своей_ветки(задачи)"** и работаем в ней

\2.   Когда задача сделана: 
 **git add -A, git commit -m "какие измения были внесены\какая задача решена"**

\3.   Переходим на основную ветку **git checkout main** и притягиваем все изменения с **remote** репозитория - **git pull origin main**

\4.   Возвращаемся в свою ветку - **git checkout название_своей_ветки(задачи**)

\5.   Производим объединение **основной** ветки со **своей локальной** веткой командой **- git merge main. ( main -> в свою)**

\6.   У себя локально решаем конфликты в коде со своей командой, если таковые возникают.

\7.   Обязательно запускаем проект, проверяем работает ли то, что вы создали, не сломали ли вы то, что уже работало. Если все работает, - идете к следующему шагу. Если нет - ищите ошибки, решаете их (если надо - привлекайте команду ) и только после этого переходите к следующему шагу.

\8.   Комитим и пушим свою ветку на сервер - **
 git add -A, git commit -m "какие измения были внесены\какая задача решена", 
 git push origin название_своей_ветки(задачи)**

\9.   на сайте [github.com](http://github.com/) делайте слияние своей ветки с основной веткой: на желтом фоне с обновлениями есть кнопка (Compare and Pull Request ), нажимаете ее и при переходе **ВНИМАНИЕ** вы выбираете откуда(справа) и куда(слева). Слева должно стоять **main**, справа - название вашей ветки (при этом справа должна появиться надпись "Able to merge". Желательно прописать название по типу "какие измения были внесены\какая задача решена". Нажимаете внизу кнопку Create pull request.

\10. Делаете мердж. После мерджа в той же форме удаляем свою ветку **delete branch**

\11. Сообщаем своей команде, что основная ветка обновлена и они могут пулить(подтягиваеть) ее себе. Все радуются и пулят.

\12. Возвращаемся в VSCode или Webshtorm. переходим в основную ветку - **git checkout main** подтягиваем изменения: **git pull origin main**

\13. Кто пользует ВСкод - запускаем команду **git fetch --prune** - она удалит персональные ветки, которые были удалены на гите. Кто использует Шторм - при клике на ветку увидит возможность удаления ветки.

\14. Берем новую задачу, и находясь в **основной** ветке, создаем и переключаемся на свою новую ветку (пункт 1) и творим дальше.

 

 