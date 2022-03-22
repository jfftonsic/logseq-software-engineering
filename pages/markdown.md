- cheatsheet table using pure markdown.
  span html tags are used to bypass markdown formatting on the `Markdown Syntax` column
	- | Element                                        | Markdown Syntax                                                                               |
	  |||
	  | Fenced Code Block                              | <span>```<br/>{<br/>&nbsp;&nbsp;"firstName": "John",<br/>&nbsp;&nbsp;"lastName": "Smith",<br/>&nbsp;&nbsp;"age": 25<br/>}<br/>```</span> |
	  | Footnote                                       | <span>Here's a sentence with a footnote. [^1]<br/><br/>[^1]: This is the footnote.                           </span> |
	  | Heading ID  (anchor links)                                    | <span>### My Great Heading {#custom-id}                                                            </span> |
	  | Definition List                                | <span>term<br/>: definition                                                                             </span> |
	  | Strikethrough                                  | <span>~~The world is flat.~~                                                                       </span> |
	  | Task List                                      | <span>- [x] Write the press release<br/>- [ ] Update the website<br/>- [ ] Contact the media                 </span> |
	  | Emoji(see also&nbsp;Copying and Pasting Emoji) | <span>That is so funny! :joy:                                                                      </span> |
	  | Highlight                                      | <span>I need to highlight these ==very important words==.                                          </span> |
	  | Subscript                                      | <span>H~2~O                                                                                        </span> |
	  | Superscript                                    | <span>X^2^                                                                                         </span> |
- cheatsheet table using the plugin [[logseq-tablerender-plugin]]
	- {{renderer :tables_jvbkjteqhj}}
		- data nosum
			- Element
				- Fenced Code Block
				- Footnote
				- Heading ID (anchor links)
				- Definition List
				- Strikethrough
				- Task List
				- Emoji
				- Highlight
				- Subscript
				- Superscript
			- Syntax
				- ```{&nbsp;&nbsp;"firstName": "John",&nbsp;&nbsp;"lastName": "Smith",&nbsp;&nbsp;"age": 25}```
				- Here's a sentence with a footnote. [^1][^1]: This is the footnote.
				- ### My Great Heading {#custom-id}
				- term
				  : definition
				- ~~The world is flat.~~
				- [x] Write the press release- [ ] Update the website- [ ] Contact the media
				- That is so funny! :joy:
				- I need to highlight these ==very important words==.
				- H~2~O
				- X^2^