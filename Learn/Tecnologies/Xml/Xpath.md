
# What is XPath

XPath (XML Path Language) is a query language for selecting nodes from an XML document. 

In it's basic form XPath allows a 'query' or 'expression' to be appied to an XML tree or branch of an XML tree. The expression can select portions of the tree based on tests applied to nodes within the tree, or can simply answer questions such as 'does the <product...> tag have a 'state' attribute with the value 'inStock'.

In addition, XPath may be used to compute values (e.g., strings, numbers, or Boolean values) from the content of an XML document. 

XPath was defined by the World Wide Web Consortium (W3C)

# XPath Examples

Given this xml

```
<root xmlns:foo="http://www.foo.org/" xmlns:bar="http://www.bar.org">
	<actors>
		<actor id="1">Christian Bale</actor>
		<actor id="2">Liam Neeson</actor>
		<actor id="3">Michael Caine</actor>
	</actors>
	<foo:singers>
		<foo:singer id="4">Tom Waits</foo:singer>
		<foo:singer id="5">B.B. King</foo:singer>
		<foo:singer id="6">Ray Charles</foo:singer>
	</foo:singers>
</root>
```

* Select the document node

```
/
```

* Select the 'root' element

```
/root
```

* Select all 'actor' elements that are direct children of the 'actors' element.

```
/root/actors/actor
```

* Select all 'singer' elements regardless of their positions in the document.

```
//foo:singer
```

* Select the 'id' attributes of the 'singer' elements regardless of their positions in the document.

```
//foo:singer/@id
```

* Select the textual value of first 'actor' element.

```
//actor[1]/text()
```

* Select the last 'actor' element.

```
//actor[last()]
```

* Select the first and second 'actor' elements using their position.

```
//actor[position() < 3]
```

* Select all 'actor' elements that have an 'id' attribute.

```
//actor[@id]
```

* Select the 'actor' element with the 'id' attribute value of '3'.

```
//actor[@id='3']
```

* Select all 'actor' nodes with the 'id' attribute value lower or equal to '3'.

```
//actor[@id<=3]
```

* Select all the children of the 'singers' node.

```
/root/foo:singers/*
```

* Select all the elements in the document.

```
//*
```

* Select all the 'actor' elements AND the 'singer' elements.

```
//actor|//foo:singer
```

* Select the name of the first element in the document.

```
name(//*[1])
```

* Select the numeric value of the 'id' attribute of the first 'actor' element.

```
number(//actor[1]/@id)
```

* Select the string representation value of the 'id' attribute of the first 'actor' element.

```
string(//actor[1]/@id)
```

* Select the length of the first 'actor' element's textual value.

```
string-length(//actor[1]/text())
```

* Select the local name of the first 'singer' element, i.e. without the namespace.

```
local-name(//foo:singer[1])
```

* Select the number of 'singer' elements.

```
count(//foo:singer)
```

* Select the sum of the 'id' attributes of the 'singer' elements.

```
sum(//foo:singer/@id)
```


# Resources

- [Xpath from wikipedia](https://en.wikipedia.org/wiki/XPath)
- [Xpath tester online](http://www.freeformatter.com/xpath-tester.html)
