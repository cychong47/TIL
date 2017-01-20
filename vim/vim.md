# bootstrap

Easily generate `.vimrc`

Create `generate.vim` from http://www.vim-bootstrap.com

Starting vim will automatically install plugins. If it failed, try this

```
vim +PlugInstall +qall
```

If curl has some issue of certificate, add `-k` option in curl command

```
if !filereadable(vimplug_exists)
  echo "Installing Vim-Plug..."
  echo ""
  silent !\curl -k -fLo ~/./autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  let g:not_finish_vimplug = "yes"

  autocmd VimEnter * PlugInstall
endif
```
