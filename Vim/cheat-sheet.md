# Vim Cheat Sheet

`h j k l`	cursor  (left, down, up, right)
`:q`	quit  
`:q!`	forced quit  
`:wq`	save (write file to disc) and quit  
`:w`	save file (as <filename>)  
`i`	insert  
`a`	append  
`A`	append at the end of the line  
`x`	delete  
`y`	yank (copy)  
`p`	put (paste)  
`r`	replace one character (`r<char>`)  
`R`	replace multiple characters (via insert mode, but replacing characters rather than inserting them)
`c`	change (uses the same motions as delete)  
  
`dw`	delete word (and everything up to the next word)  
`de`	delete only up to the end of the word  
`d$`	delete (from here) to the end of the line  
`dd`	delete a whole line (stored in a vim register)  
`u`	  undo  
`U`	  undo all changes to a line  
`str-R`	undo the undo's  
  
`<num>w`	move <num> words forward  
`<num>e`	move forward to the end of <num> words  
`0`	      move to start of the line  
`d<num>`	delete a number of words etc.  
  
`ctrl-g`	show file status and location in file  
`G`	go to end of file  
`<line>G`	go to a particular line   
`gg`	go to beginning of file  

`/`		search a phrase (`?` for opposite direction)  
`n` or `N`		search the phrase again (up or down)  
`%`		find matching parantheses  
`:set ic`		ignore case  
`:set noic`	disable ic  
`:set hls`	hlsearch (highlight search matches)  
`:nohls`		disable hls  
`:set is`		incsearch (incremental searches - shows partial matches for a search phrase)  
`:set nois`	disable  is
  
`:s/<old>/<new>`		substitute a word, add `/g` for global substitution within the line  
`:<line-number1>,<line-number2>s/<old>/<new>/g`	substitutes every occurence within the range of these lines  
`:%s/<old>/<new>/g`	sub every occurence within the whole file  
`:%s/<old>/<new>/gc`	- " - with a prompt whether to substitute or not  
  
`:!`	execute external commands  
  
`v`	visual mode, let's you select text and operate with this selection  
`:r`	retrieve (read) a file and place it under the cursor line (can also retrieve a command output)  
`o` or `O`	open a line (in insert mode) under or above the cursor  
  
`:<char> ctrl-d`	show a list of commands that start with <char>  
`ctrl-w`		jump to another window  
`:help`		vim help  
`:help <cmd>`	command help  