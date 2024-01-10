# Руководство использования системы VCS Git
*VCS Git* - **система (программа), позволяющая смотреть, контролировать и анализировать изменения в проекте.**

## Шаг №0 (Начало работы с Git)
Чтобы приступить к работе с VCS Git необходимо установить специализированный пакет с официального сайта или же воспользоваться терминалом.
После инсталяции пакета можно проверить установленную версию посредством CLI Bash которая устанавливается вместе с Git пакетом. [Сайт VCS Git](https://git-scm.com/)
В случае с Mac и Linux команду можно выполнять прямо в терминале, для этого нужно ввести команду `git version`. 
Если есть необходимость произвести обновление, то это можно сделать командой `git update-git-for-windows" в случае с Windows или просто `git update`.

## Шаг #1 (Разворачивание локального репозитория)
Теперь разберем процесс инициализации системы отслеживания версий Git, для этого необходимо в CLI Bash перейти в директорию которую мы хотим сделать репозиторием посредством использования команд `ls`, `cd`, `mkdir`, `touch`.
Затем для того Git начал отслеживать необходимую нам дерикторию, нужно выполнить инициализацию репозитория командой `git init`, если в пути появилась надпись ветки (***master or main***) то репозиторий инициализирован!

### В случае если необходимо "разгитить" директорию
В случае если мы случайно проинициализировали не ту папку или просто не нуждаемся больше в отслеживании изменений то необходимо перейти в необходимую директорию посредством `cd <папка с репозиторием>` и выполнить несложную команду с флагами -rf: `rm -rf .git`, только нужно пользоваться ею аккуратно, дабы не удалить все файлы.

Важная и полезная команда `git status`, с помощью ее можно и нужно смотреть информацию по репозиторию, например какие файлы еще не отслежены, проинициализирован ли репозиторий вообще и т.д.

Далее, для того чтобы зафиксировать версии файлов в репозитории необходимо воспользоваться командой `git add ./` или `git add <имя файла>`.
Таким образом организуется отслеживание изменений в локальном репозитории, при желании мы можем просматривать изменения командой `git log`

## Шаг №2 (Организация доступа к удаленному хранилищу репозиторий)
Последующим шагом необходимо соеденить наше устройство с аккаунтом удаленного хранилища репозиториев (GitHub и т.п.)

Прежде чем начать, необходимо настроить информацию о клиенте Git если вы этого ранее не сделали, а именно указать email и имя, для этого необходимо воспользовать командами `git config --global user.name "Имя или ник"` и `git config --global user.email <ваш рабочий или личный email>`.

Для начала нужно проверить наличие ключей SSH, перейдем в домашний раздел `cd ~` и введем команду `ls -la .ssh/`, в случае если там есть файлы с разрешением *.pub*, а вы их не делали то удалите их.

### Генерация ключа SSH
Для генерации SSH-пары можно использовать терминальную программу *ssh-keygen*. В терминале Bash пропишем следующую команду:
`$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на VCS"` Используйте электронную почту, к которой привязан ваш GitHub-аккаунт.

Если вы видите сообщение об ошибке, то, скорее всего, ваша система не поддерживает алгоритм шифрования ed25519. Ничего страшного: используйте другой алгоритм.
`$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"`

### Привязывание профиля VCS и клиента по SSH
Затем когда ключ мы уже создали, его необходимо привязать к GitHub, для этого необходимо вытащить публичный ключ **(ВАЖНО! Никому не сообщать приватный ключ!!!!)** `clip или cat < ~/.ssh/ключ.pub`, после чего зайти в настройки профиля VCS, найти пункт SSH and GPG keys и добавить ключ SSH, в строку SSH Key необходимо вставить полный текст скопированный из сгенерированного ключа SSH.

Для проверки связи с GitHub или иным средством хранения репозиторием воспользуемся командой `ssh -T git@github.com`, проверяем подключены ли мы к серверам GitHub. При первом подключении сервис VCS может запросить подтверждение верификации!

## Шаг №3 (Создание коммита и отправка его на удаленный сервер)
После добавления файлов к отслеживанию и подключения клиента к сервисам VCS можно переходить к *фиксации* изменений и отправке их на удаленный репозиторий сервера VCS.

### Привязка локального репозитория за удаленным репозиторием
Для начала необходимо перейти в Web-версию VCS и создать в своем профиле *открытый* или *закрытый* удаленный репозиторий через предложенный пользовательский интерфейс сайта.

После его создания мы уже можем закрепить локальный репозиторий за созданным удаленным, посредством использования команды `git remote add origin git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ_РЕПОЗИТОРИЯ%.git`
Для проверки связи локального репозитория используем `git remote -v`.

### Создание Commit'a и отправка его на удаленнный репозиторий
Для того чтобы закоммитить все ранее добавленные файлы из *шага №1* (зафиксировать в истории ветки), необходимо воспользовать командой: `git commit -m <Информация о изменении>`.
Если появится необходимость посмотреть информацию о коммитах проекта, то воспользуемся командой: `git log`.

Теперь когда мы полностью все "зафиксировали", можно приступить к отправке данных на удаленный репозиторий командой: `git push -u origin %Название ветки%`. **Важная ремарка!** Изначально в репозитории создается ветка *main*, однако раньше таковой была ветка *master*, но в следствии истории начальную ветку переименовали. 

## Навигация по коммитам
В разработке

~~Данное руководство находится в разработке, с улучшением знаний оно будет форматироваться!~~
