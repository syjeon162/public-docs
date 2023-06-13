
## Save and Quit

Save

	:w

Quit

	:q

Save and Quit

	:wq

Force Quite without saving changes

	:q!

## Edit Modes
go into insert (edit) mode

	i

go into visual mode

	v

in visual mode (and visual block mode), press `d` to delete anything that was selected visually.

go into visual block mode

	ctrl + v

in visual block mode, after selecting multiple lines, you can press `shift + i` to insert same text at same position in multiple lines. 
It will appear as if you are only inserting text in one line, but after pressing `esc` twice, it will be inserted in the rest.
This is useful for commenting out multiple lines at once.

to get out of any mode

	esc

## Navigation

go to first line of document

	g

go to 5th (or any) line of document

	5gg

jump to first non-blank character of line

	^

jump to end of line

	$

## Delete

delete a line

	dd

delete 2 (or more) lines

	2dd

you can also do something like `100dd`, it will delete 100 lines.

delete multiple lines from line number A to B

	:A,Bd

## Find and Replace

search for word

	/WORD

to escape some characters (ex. `/`, `.`), add a `\` in front of it.

move to next searched word

	n

move to previous searched word

	N

find and replace words in entire document

	:%s/ORIGINAL/REPLACEMENT/g

## Copy and Paste
yank (copy) a line

	yy

yank (copy) 2 (or more) lines

	2yy

paste

	p