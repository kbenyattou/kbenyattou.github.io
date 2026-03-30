---
layout: page
title: "Test Test Test"
permalink: /test/
---

SQL
<pre><code class="language-sql">SELECT column_list
	, column_2
	--, easy to comment out
FROM table_name
JOIN second_table_name ON expression_for_column_in_common
WHERE filter_condition LIKE '\%word\%'
GROUP BY grouping
HAVING aggregate_filter_condition
ORDER BY column_list [ASC|DESC]
LIMIT number_of_rows;

COUNT() OR something

window_function() OVER (
                    [PARTITION BY partition_expression,]
                    [ORDER BY sort_expression [ASC|DESC],]
                    [ROWS BETWEEN frame_start AND frame_end]
                        )
</code></pre>

Python
<pre><code class="language-python">class ClassName:
	def __init__(self, name):
		self.name = "Keb"
	def __str__(self):
		return f"{self.name}"
	@classmethod
	@staticmethod
	
if x not in [(0,1), (-4,2)]:
	raise ValueError("It\'s not in there")
	raise AssertionError("yes")

>>> True
>>> None
x>>y
str(1)

{a,b}
[a,b]
y&x
y^x
y~x
x+=y
x<y
x=y
5%2 yes

'Single quote string'
"Double quote string"
"""Testing the triple
quote on multiple lines"""
'''Triple line
single quotes'''</code></pre>

Diff
<pre><code class="language-diff">- this line was removed
+ this line was added
</code></pre>