1. `brew install macvim`
	- The `vim` that came with MacOS (located at `/usr/bin/vim`) did not have `python3` or `clientserver` installed. `macvim` is more batteries-included. Run `vim --version` to check what features your `vim` includes 
2. `brew tap zegervdv/zathura` and `brew install zathura --with-synctex` and `brew install zathura-pdf-poppler`
	- This is the PDF viewer I use
	- To get these to install, I also had to `brew install gcc` and upgrade my Xcode Command Line Tools via MacOS's software update
	- To enable the `zathura-pdf-poppler` plugin, run

```
mkdir -p $(brew --prefix zathura)/lib/zathura
ln -s $(brew --prefix zathura-pdf-poppler)/libpdf-poppler.dylib $(brew --prefix zathura)/lib/zathura/libpdf-poppler.dylib
```
3. `brew install mactex-no-gui`
	- You need some sort of TeX compiler for VimTeX to work. This is the one I installed

4. Install [vim-plug](https://github.com/junegunn/vim-plug). Add the `call plug#begin(~/.vim/plugged')` and `call plug#end()` lines to your `.vimrc` as recommended in the README
5. Install [VimTeX](https://github.com/lervag/vimtex) using `vim-plug` as your plugin manager. Here's my `.vimrc` code:

```
Plug 'lervag/vimtex'
    let g:tex_flavor='latex'
    let g:vimtex_view_method='zathura'
    let g:vimtex_quickfix_mode=0
    set conceallevel=1
    let g:tex_conceal='abdmg'
```

6. Reload `.vimrc` and run `:PlugInstall` in `vim` to perform installation

At this point, you should be able to edit a `.tex` file with vim and see zathura display the compiled PDF every time you save the file. You may have to type `\ll` to get the continuous TeX compiler to start up the first time you open the file.
## Bonus: Snippets and TeX conceal
7. Install [UltiSnips](https://github.com/SirVer/ultisnips) by adding this to your `.vimrc`

```
Plug 'sirver/ultisnips'
    let g:UltiSnipsExpandTrigger = '<tab>'
    let g:UltiSnipsJumpForwardTrigger = '<tab>'
    let g:UltiSnipsJumpBackwardTrigger = '<s-tab>'
```

8. Install [tex-conceal](https://github.com/KeitaNakamura/tex-conceal.vim) by adding this to your `.vimrc`

```
Plug 'KeitaNakamura/tex-conceal.vim'
    set conceallevel=1
    let g:tex_conceal='abdmg'
    hi Conceal ctermbg=none
```

9. Add `UltiSnips` snippets to your heart's content in your `~/.vim/UltiSnips/tex.snippets` file. I used [this guide](https://castel.dev/post/lecture-notes-1/) to learn about and develop snippets that I use.