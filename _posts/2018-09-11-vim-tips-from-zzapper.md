---
layout: post
title: Vim tips from zzapper
date: 2018-09-11 22:08 +0800
categories: [Miscelaneous]
tags: [vim, tips]
---

## Best of Vim Tips
zzapper 16 Years of Vi + 10+ years of Vim and still learning
31Aug18 : Last Update (Now in VIM Help Format :h helptags)  
###These Tips are now being maintained at [zzapper.co.uk/vimtips.html](
http://zzapper.co.uk/vimtips.html)

```vim
__BEGIN__
*vimtips.txt*	For Vim version 8.0.  
------------------------------------------------------------------------------
" new items marked [N] , corrected items marked [C]
" *best-searching*
/joe/e                      : cursor set to End of match
3/joe/e+1                   : find 3rd joe cursor set to End of match plus 1 [C]
/joe/s-2                    : cursor set to Start of match minus 2
/joe/+3                     : find joe move cursor 3 lines down
/^joe.*fred.*bill/          : find joe AND fred AND Bill (Joe at start of line)
/^[A-J]/                    : search for lines beginning with one or more A-J
/begin\_.*end               : search over possible multiple lines
/fred\_s*joe/               : any whitespace including newline [C]
/fred\|joe                  : Search for FRED OR JOE
/.*fred\&.*joe              : Search for FRED AND JOE in any ORDER!
/\<fred\>/                  : search for fred but not alfred or frederick [C]
/\<\d\d\d\d\>               : Search for exactly 4 digit numbers
/\D\d\d\d\d\D               : Search for exactly 4 digit numbers
/\<\d\{4}\>                 : same thing
/\([^0-9]\|^\)%.*%          : Search for absence of a digit or beginning of line
" finding empty lines
/^\n\{3}                    : find 3 empty lines
/^str.*\nstr                : find 2 successive lines starting with str
/\(^str.*\n\)\{2}           : find 2 successive lines starting with str
" using rexexp memory in a search find fred.*joe.*joe.*fred *C*
/\(fred\).*\(joe\).*\2.*\1
" Repeating the Regexp (rather than what the Regexp finds)
/^\([^,]*,\)\{8}
" visual searching
:vmap // y/<C-R>"<CR>       : search for visually highlighted text
:vmap <silent> //    y/<C-R>=escape(@", '\\/.*$^~[]')<CR><CR> : with spec chars
" \zs and \ze regex delimiters :h /\zs
/<\zs[^>]*\ze>              : search for tag contents, ignoring chevrons
" zero-width :h /\@=
/<\@<=[^>]*>\@=             : search for tag contents, ignoring chevrons
/<\@<=\_[^>]*>\@=           : search for tags across possible multiple lines
" searching over multiple lines \_ means including newline
/<!--\_p\{-}-->                   : search for multiple line comments
/fred\_s*joe/                     : any whitespace including newline *C*
/bugs\(\_.\)*bunny                : bugs followed by bunny anywhere in file
:h \_                             : help
" search for declaration of subroutine/function under cursor
:nmap gx yiw/^\(sub\<bar>function\)\s\+<C-R>"<CR>
" multiple file search
:bufdo /searchstr/                : use :rewind to recommence search
" multiple file search better but cheating
:bufdo %s/searchstr/&/gic   : say n and then a to stop
" How to search for a URL without backslashing
?http://www.vim.org/        : (first) search BACKWARDS!!! clever huh!
" Specify what you are NOT searching for (vowels)
/\c\v([^aeiou]&\a){4}       : search for 4 consecutive consonants
/\%>20l\%<30lgoat           : Search for goat between lines 20 and 30 [N]
/^.\{-}home.\{-}\zshome/e   : match only the 2nd occurence in a line of "home" [N]
:%s/home.\{-}\zshome/alone  : Substitute only the 2nd occurrence of home in any line [U]
" find str but not on lines containing tongue
^\(.*tongue.*\)\@!.*nose.*$
\v^((tongue)@!.)*nose((tongue)@!.)*$
.*nose.*\&^\%(\%(tongue\)\@!.\)*$ 
:v/tongue/s/nose/&/gic
'a,'bs/extrascost//gc       : trick: restrict search to between markers (answer n) [N]
/integ<C-L>                 : Control-L to complete search term [N]
"----------------------------------------
" *best-substitution*
:%s/fred/joe/igc            : general substitute command
:%s//joe/igc                : Substitute what you last searched for [N]
:%s/~/sue/igc               : Substitute your last replacement string [N]
:%s/\r//g                   : Delete DOS returns ^M
" Is your Text File jumbled onto one line? use following
:%s/\r/\r/g                 : Turn DOS returns ^M into real returns
:%s=  *$==                  : delete end of line blanks
:%s= \+$==                  : Same thing
:%s#\s*\r\?$##              : Clean both trailing spaces AND DOS returns
:%s#\s*\r*$##               : same thing
" deleting empty lines
:%s/^\n\{3}//               : delete blocks of 3 empty lines
:%s/^\n\+/\r/               : compressing empty lines
:%s#<[^>]\+>##g             : delete html tags, leave text (non-greedy)
:%s#<\_.\{-1,}>##g          : delete html tags possibly multi-line (non-greedy)
:%s#.*\(\d\+hours\).*#\1#   : Delete all but memorised string (\1) [N]
" parse xml/soap 
%s#><\([^/]\)#>\r<\1#g      : split jumbled up XML file into one tag per line [N]
%s/</\r&/g                  : simple split of html/xml/soap  [N]
:%s#<[^/]#\r&#gic           : simple split of html/xml/soap  but not closing tag [N]
:%s#<[^/]#\r&#gi            : parse on open xml tag [N]
:%s#\[\d\+\]#\r&#g          : parse on numbered array elements [1] [N]
ggVGgJ                      : rejoin XML without extra spaces (gJ) [N]
%s=\\n#\d=\r&=g             : parse PHP error stack [N]
:%s#^[^\t]\+\t##            : Delete up to and including first tab [N]
" VIM Power Substitute
:'a,'bg/fred/s/dick/joe/igc : VERY USEFUL
" duplicating columns
:%s= [^ ]\+$=&&=            : duplicate end column
:%s= \f\+$=&&=              : Dupicate filename
:%s= \S\+$=&&               : usually the same
" memory
:%s#example#& = &#gic        : duplicate entire matched string [N]
:%s#.*\(tbl_\w\+\).*#\1#    : extract list of all strings tbl_* from text  [NC]
:s/\(.*\):\(.*\)/\2 : \1/   : reverse fields separated by :
:%s/^\(.*\)\n\1$/\1/        : delete duplicate lines
:%s/^\(.*\)\(\n\1\)\+$/\1/  : delete multiple duplicate lines [N]
" non-greedy matching \{-}
:%s/^.\{-}pdf/new.pdf/      : delete to 1st occurence of pdf only (non-greedy)
%s#^.\{-}\([0-9]\{3,4\}serial\)#\1#gic : delete up to 123serial or 1234serial [N]
" use of optional atom \?
:%s#\<[zy]\?tbl_[a-z_]\+\>#\L&#gc : lowercase with optional leading characters
" over possibly many lines
:%s/<!--\_.\{-}-->//        : delete possibly multi-line comments
:help /\{-}                 : help non-greedy
" substitute using a register
:s/fred/<c-r>a/g            : sub "fred" with contents of register "a"
:s/fred/<c-r>asome_text<c-r>s/g  
:s/fred/\=@a/g              : better alternative as register not displayed (not *) [C]
:s/fred/\=@*/g              : replace string with contents of paste register [N]
" multiple commands on one line
:%s/\f\+\.gif\>/\r&\r/g | v/\.gif$/d | %s/gif/jpg/
:%s/a/but/gie|:update|:next : then use @: to repeat
" ORing
:%s/goat\|cow/sheep/gc      : ORing (must break pipe)
:'a,'bs#\[\|\]##g           : remove [] from lines between markers a and b [N]
:%s/\v(.*\n){5}/&\r         : insert a blank line every 5 lines [N]
" Calling a VIM function
:s/__date__/\=strftime("%c")/ : insert datestring
:inoremap \zd <C-R>=strftime("%d%b%y")<CR>    : insert date eg 31Jan11 [N]
" Working with Columns sub any str1 in col3
:%s:\(\(\w\+\s\+\)\{2}\)str1:\1str2:
" Swapping first & last column (4 columns)
:%s:\(\w\+\)\(.*\s\+\)\(\w\+\)$:\3\2\1:
" format a mysql query 
:%s#\<from\>\|\<where\>\|\<left join\>\|\<\inner join\>#\r&#g
" filter all form elements into paste register
:redir @*|sil exec 'g#<\(input\|select\|textarea\|/\=form\)\>#p'|redir END
:nmap ,z :redir @*<Bar>sil exec 'g@<\(input\<Bar>select\<Bar>textarea\<Bar>/\=form\)\>@p'<Bar>redir END<CR>
" substitute string in column 30 [N]
:%s/^\(.\{30\}\)xx/\1yy/
" decrement numbers by 3
:%s/\d\+/\=(submatch(0)-3)/
" increment numbers by 6 on certain lines only
:g/loc\|function/s/\d/\=submatch(0)+6/
" better
:%s#txtdev\zs\d#\=submatch(0)+1#g
:h /\zs
" increment only numbers gg\d\d  by 6 (another way)
:%s/\(gg\)\@<=\d\+/\=submatch(0)+6/
:h zero-width
" rename a string with an incrementing number
:let i=10 | 'a,'bg/Abc/s/yy/\=i/ |let i=i+1 # convert yy to 10,11,12 etc
" as above but more precise
:let i=10 | 'a,'bg/Abc/s/xx\zsyy\ze/\=i/ |let i=i+1 # convert xxyy to xx11,xx12,xx13
" find replacement text, put in memory, then use \zs to simplify substitute
:%s/"\([^.]\+\).*\zsxx/\1/
" Pull word under cursor into LHS of a substitute
:nmap <leader>z :%s#\<<c-r>=expand("<cword>")<cr>\>#
" Pull Visually Highlighted text into LHS of a substitute
:vmap <leader>z :<C-U>%s/\<<c-r>*\>/
" substitute singular or plural
:'a,'bs/bucket\(s\)*/bowl\1/gic   [N]
----------------------------------------
" all following performing similar task, substitute within substitution
" Multiple single character substitution in a portion of line only
:%s,\(all/.*\)\@<=/,_,g     : replace all / with _ AFTER "all/"
" Same thing
:s#all/\zs.*#\=substitute(submatch(0), '/', '_', 'g')#
" Substitute by splitting line, then re-joining
:s#all/#&^M#|s#/#_#g|-j!
" Substitute inside substitute
:%s/.*/\='cp '.submatch(0).' all/'.substitute(submatch(0),'/','_','g')/
----------------------------------------
" *best-global* command 
:g/gladiolli/#              : display with line numbers (YOU WANT THIS!)
:g/fred.*joe.*dick/         : display all lines fred,joe & dick
:g/\<fred\>/                : display all lines fred but not freddy
:g/^\s*$/d                  : delete all blank lines
:g!/^dd/d                   : delete lines not containing string
:v/^dd/d                    : delete lines not containing string
:g/joe/,/fred/d             : not line based (very powerfull)
:g/fred/,/joe/j             : Join Lines [N]
:g/-------/.-10,.d          : Delete string & 10 previous lines
:g/{/ ,/}/- s/\n\+/\r/g     : Delete empty lines but only between {...}
:v/\S/d                     : Delete empty lines (and blank lines ie whitespace)
:v/./,/./-j                 : compress empty lines
:g/^$/,/./-j                : compress empty lines
:g/<input\|<form/p          : ORing
:g/^/put_                   : double space file (pu = put)
:g/^/m0                     : Reverse file (m = move)
:g/^/m$                     : No effect! [N]
:'a,'bg/^/m'b               : Reverse a section a to b
:g/^/t.                     : duplicate every line
:g/fred/t$                  : copy (transfer) lines matching fred to EOF
:g/stage/t'a                : copy (transfer) lines matching stage to marker a (cannot use .) [C]
:g/^Chapter/t.|s/./-/g      : Automatically underline selecting headings [N]
:g/\(^I[^^I]*\)\{80}/d      : delete all lines containing at least 80 tabs
" perform a substitute on every other line
:g/^/ if line('.')%2|s/^/zz / 
" match all lines containing "somestr" between markers a & b
" copy after line containing "otherstr"
:'a,'bg/somestr/co/otherstr/ : co(py) or mo(ve)
" as above but also do a substitution
:'a,'bg/str1/s/str1/&&&/|mo/str2/
:%norm jdd                  : delete every other line
" incrementing numbers (type <c-a> as 5 characters)
:.,$g/^\d/exe "norm! \<c-a>": increment numbers
:'a,'bg/\d\+/norm! ^A       : increment numbers
" storing glob results (note must use APPEND) you need to empty reg a first with qaq. 
"save results to a register/paste buffer
:g/fred/y A                 : append all lines fred to register a
:g/fred/y A | :let @*=@a    : put into paste buffer
:g//y A | :let @*=@a    : put last glob into paste buffer [N]
:let @a=''|g/Barratt/y A |:let @*=@a
" filter lines to a file (file must already exist)
:'a,'bg/^Error/ . w >> errors.txt
" duplicate every line in a file wrap a print '' around each duplicate
:g/./yank|put|-1s/'/"/g|s/.*/Print '&'/
" replace string with contents of a file, -d deletes the "mark"
:g/^MARK$/r tmp.txt | -d
" display prettily
:g/<pattern>/z#.5           : display with context
:g/<pattern>/z#.5|echo "=========="  : display beautifully
" Combining g// with normal mode commands
:g/|/norm 2f|r*                      : replace 2nd | with a star
"send output of previous global command to a new window
:nmap <F3>  :redir @a<CR>:g//<CR>:redir END<CR>:new<CR>:put! a<CR><CR>
"----------------------------------------
" *Best-Global-combined-with-substitute* (*power-editing*)
:'a,'bg/fred/s/joe/susan/gic :  can use memory to extend matching
:/fred/,/joe/s/fred/joe/gic :  non-line based (ultra)
:/biz/,/any/g/article/s/wheel/bucket/gic:  non-line based [N]
----------------------------------------
" Find fred before beginning search f
```
