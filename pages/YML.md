- Multiline text
	- ```
	   string1: |
	     Line1
	     line2
	     "line3"
	    line4
	    
	   or
	   
	   string1: >
	     Line1
	     line2
	     "line3"
	    line4
	  
	   With | The new line is preserved and Leading spaces or indentations are removed.
	   With > newlines are not preserved, newlines are converted to spaces, Leading (beginning) indentation or spaces of a
	    line are not considered, Trailing or end of line spaces are not preserved
	  ```