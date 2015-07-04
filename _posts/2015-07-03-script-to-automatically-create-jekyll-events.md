---
layout: post
title: Script to automatically create Jekyll events
categories: Jekyll, Bash
---

Here's a script I edited, originally from [rigelk's](https://gist.github.com/rigelk) [script](https://gist.github.com/rigelk/8770430).  It makes a new, blank Jekyll template for a blog post in the _posts folder.  It takes in a post title and categories, and is much better than manually creating the file yourself.  The original code didn't finish the if statement, so I just added that.

Here's the [gist](https://gist.github.com/syntactical/e1bbf6850a54254c6afe):

	usage="USAGE ./new_event [editor]"
	if [ -z "$1" ]
	then
	        read -r -p "Post name > "
	        title=${REPLY}
	        read -r -p "Category > "
	        cat=${REPLY}
	        title_clean="$(<<< "$title" \
	        iconv -f utf8 -t ascii//translit \
	        | tr ' [:upper:]' '-[:lower:]' \
	        | tr -dc 'a-z0-9._-')"

	        filename="_posts/$(date "+%Y-%m-%d")-$title_clean.md"
	        echo "---
	layout: post
	title: $title
	categories: $cat
	---
	" > "$filename"
	fi
