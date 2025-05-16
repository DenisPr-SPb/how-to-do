# Установка  zsh и Oh-my-zsh + мои alias (Fedora 42)
### Обновим систему
```bash
  sudo dnf update && sudo dnf upgrade -y 
```
### Устанавливаем zsh
```bash
    sudo dnf install zsh
```
### Устанавливаем Oh-my-zsh
* Заходим на официальный сайт [Oh-my-zsh](https://ohmyz.sh/)
* Находим скрипт загрузки и копируем его
```bash
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
### Aliases
Обновим файл конфигурации .zshrc
```bash
    sudo nano ~/.zshrc
```
Добавляем эти алиасы в самый конец файла .zshrc
```
    alias goup="sudo dnf update && sudo dnf upgrade -y"
    alias sdi="sudo dnf install"
    alias cc="clear"
    alias neo="neofetch"
```
* Ctrl + O - созраняем изменения
* Ctrl + X - выходим из редактора nano
### Что бы изменения начали работать
```bash
    source ~/.zshrc
```