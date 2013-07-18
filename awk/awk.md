1. My convention:
	* 每个statement后面都加上分号
	* 不求cmd有多精简，但求易读并且能解决问题，即使有时候很繁琐
2. Some abbrev:
	* $0代表一整行
	* 1或者'1'或者'1;'代表'{print}'
3. Some functions:
	* sub(regex, repl, [string]) 
		* 如果string missing, 默认为$0
		* 只是代替第一个regex匹配的字符串
	* gsub(regex, repl, [string])
		* perform as many substitutions as possible
	* printf
	* print
		* print a,t不同于print a t 前者会在两者之间多打印一空格
	* gensub(regex, repl, h[, string])
		* awk '{gensub(/foo/, "bar", 4)}{print}'
			* replaces the 4th instance of "foo" with "bar" and assign the new string back to the whole line $0
	* getline t
		* reads the next line from input and stores it int variable t
3. Pattern-Action statements ("pattern {action statements}")
	* In a pattern-action statement either the pattern or the action may be missing.
		* pattern missing: the action statements are applied to every single line
		* statement missing: { print }
	* some examples
	```awk
	awk '1'
	awk '1;'
	awk '1; {print "";}'
	awk '1{print}{print "";}'
	awk 'BEGIN {ORS="\n\n"};1' 
	```
4. some examples:
	```awk
	awk '{sub(/^[ \t]+/,""); print}'
	awk '{gsub(/[ \t]+/, "");print}'
	awk '{sub(/^[ \t]+ | [ \t]+$/, "");print}'
	awk '{print "   ", $0}'
	awk '/baz/ {sub(/foo/, "baz")} {print}'
	awk '!/baz/ {sub(/foo/, "baz")} {print}'
	awk '{gsub(/that|those|this/,"the")}{print}'
	
	# The following 2 are equivalent
	awk -F ':' '{print $0 | "sort"}'
	awk 'BEGIN{FS=":"}{print $0 | "sort"}'
	```
5. Idiomatic awk
	* The power of conditions
		```awk
		# If all u want to do is print some lines, 
		# u can wirte awk programs composed only of a 
		# condition (complex sometimes)
		awk '{if (/pattern/)print $0}'
		awk '/pattern/ {print $0}'
		awk '/pattern/ {print}'
		awk '/pattern/'
		
		awk 'NR % 6'            # prints all lines except lines 6,12,18...
		awk 'NR > 5'            # prints from line 6 onwards (like tail -n +6, or sed '1,5d')
		awk '$2 == "foo"'       # prints lines where the second field is "foo"
		awk 'NF >= 6'           # prints lines with 6 or more fields
		awk '/foo/ && /bar/'    # prints lines that match /foo/ and /bar/, in any order
		awk '/foo/ && !/bar/'   # prints lines that match /foo/ but not /bar/
		awk '/foo/ || /bar/'    # prints lines that match /foo/ or /bar/ (like grep -e 'foo' -e 'bar')
		awk '/foo/,/bar/'       # prints from line matching /foo/ to line matching /bar/, inclusive
		awk 'NF'                # prints only nonempty lines (or: do not print empty lines, where NF==0)
		awk NF                  # prints only nonempty lines (or: do not print empty lines, where NF==0)
		awk 'NF--' a             # removes last field and prints the line
		awk '$0 = NR" "$0'      # prepends line numbers (assignments are valid in conditions)
		awk '!a[$0]++'          # suppresses duplicated lines! (figure out how it works)
		```
		
5. Reference
	* http://www.catonmat.net/download/awk.cheat.sheet.pdf
	* http://www.catonmat.net/blog/awk-one-liners-explained-part-two/
	* http://backreference.org/2010/02/10/idiomatic-awk/
	* http://www.catonmat.net/blog/ten-awk-tips-tricks-and-pitfalls/
