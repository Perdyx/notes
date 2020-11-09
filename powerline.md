# Powerline

## Debian

`sudo apt install powerline fonts-powerline`

## Vim

`sudo apt install vim-airline`

Then, add the following to your .vimrc:

```
let g:airline_powerline_fonts=1
set ttimeoutlen=10
```

## Tmux

Put the following at the top of your tmux config:

```
run-shell "powerline-daemon -q"
source "{repository_root}/powerline/bindings/tmux/powerline.conf"
```

## Zsh

[https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions](https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions)
