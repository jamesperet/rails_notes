# CSS3 Attribute Selectors (CSS selector)

Syntax
[attribute { ^= | $= | *= } attribute value] {
declaration block
}

### Description

CSS3 defines three more attribute selector variations. These new selectors give us the ability to make partial matches to attribute values—we can match strings at the start, end, or anywhere within an attribute value.

We can use the ^= operator to cause an attribute selector to match elements that have an attribute containing a value that starts with the specified value:

	a[href^="http:"] {
	  ⋮ declarations
	}

This example matches a elements that have an href attribute value which starts with the characters "http:".

Using the $= operator, an attribute selector can match elements that have an attribute which contains a value ending with the specified value:

	img[src$=".png"] {
	  ⋮ declarations
	}

This example matches img elements with a src attribute value that ends with the characters ".png".

Finally, we can use the *= operator to make an attribute selector match elements that have an attribute which contains the specified value:

	div[id*="foo"] {
	  ⋮ declarations
	}

This example matches div elements whose id attribute value contains the characters "foo".

### Example

This example will match a elements with an href attribute that contains the string "example.com":
	
	a[href*="example.com"] {
	  ⋮ declarations
	}