https://losst.pro/ustanovka-node-js-ubuntu-18-04

https://tecadmin.net/install-nvm-macos-with-homebrew/


## УСТАНОВКА NODE.JS В NODE VERSION MANAGER

Чтобы установить Node.js Ubuntu 20.04 с помощью NVM нам понадобится компилятор C++ в системе, а также другие инструменты для сборки. По умолчанию система не поставляется с этими программами, поэтому их необходимо установить. Для этого выполните команду:

 `sudo apt install build-essential checkinstall`

Также нам понадобится libssl:

 `sudo apt install libssl-dev`

Скачать и установить менеджер версий NVM можно с помощью следующей команды:

`wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash`

[![](https://losst.pro/wp-content/uploads/2016/06/nodejs2-2-890x576.png)](https://losst.pro/wp-content/uploads/2016/06/nodejs2-2.png)

После завершения установки вам понадобится перезапустить терминал. Или можно выполнить:

`source /etc/profile`

Затем смотрим список доступных версий Node js:

`nvm ls-remote`

[![](https://losst.pro/wp-content/uploads/2016/06/nodejs3-2-890x576.png)](https://losst.pro/wp-content/uploads/2016/06/nodejs3-2.png)

Дальше можно устанавливать Node js в Ubuntu, при установке обязательно указывать версию, на данный момент самая последняя 11.0, но установим десятую:

 `nvm install 14.0`

[![](https://losst.pro/wp-content/uploads/2016/06/nodejs4-2-890x576.png)](https://losst.pro/wp-content/uploads/2016/06/nodejs4-2.png)

Список установленных версий вы можете посмотреть выполнив:

 `nvm list`

[![](https://losst.pro/wp-content/uploads/2016/06/nodejs5-2-890x576.png)](https://losst.pro/wp-content/uploads/2016/06/nodejs5-2.png)