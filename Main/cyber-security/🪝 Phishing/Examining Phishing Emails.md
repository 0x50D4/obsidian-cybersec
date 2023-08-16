## Lookout!
- Senders email might not match the sender’s details.
- Unusual recipient
- Email hints at something that you haven’t done
- __Sense of urgency__
- Before entering your details into a login page, __ALWAYS__ check the URL
- Grammatical errors
- [typosquatting](https://www.csoonline.com/article/570173/what-is-typosquatting-a-simple-but-effective-attack-technique.html)
- Hiding things in [[BCC]]

## Further testing
If there is a link, we can inspect the html source of the email, and see the link without clicking it.

Chances are, it’s going to be a shortened link. You can view the destination by using [an online tool](https://checkshorturl.com/)

We can also use #python to view the destination like so:

```python
>>> import urllib
>>> resp = urllib.urlopen('http://bit.ly/bcFOko')
>>> resp.getcode()
200
>>> resp.url
'http://mrdoob.com/lab/javascript/harmony/'
```

You can also try seeing if every element is actually intractable, does the “help center” link lead anywhere?

## Tracking pixels

> [!info] Definition
> A tracking pixel is a very small image. When your browser loads the image, it sends to the site that the image has been requested, and this way the attacker knows that you’ve opened the email.

Example:
![[Pasted image 20230816143212.png]]

[For more info](https://www.theverge.com/22288190/email-pixel-trackers-how-to-stop-images-automatic-download)

> [!warning]
> When entering malicious URLs or sharing them always use [[URL Defanging]]


## Examples
We can see an email sample on [anyrun](https://app.any.run/tasks/12dcbc54-be0f-4250-b6c1-94d548816e5c/). This one uses __sense of urgency__, __poor grammar__, and makes you __log into your onedrive__.
