# Bypassing htmlentities & htmlspecialchars
It is well understood that `htmlentities()` and `htmlspecialchars()` are functions designed to mitigate XSS vulnerabilities.
Upon writing the following code, I observed how these functions prevented XSS:

```php
<?php
$a = "!@#$%^&*()}{\'\"|?><`~}";
echo "htmlentities : ".htmlentities($a)."<br>";
echo "htmlspecialchars : ".htmlspecialchars($a);
?>
```
The resulting output was:
```php
htmlentities : !@#$%^&amp;*()}{\'&quot;|?&gt;&lt;`~}
htmlspecialchars : !@#$%^&amp;*()}{\'&quot;|?&gt;&lt;`~}
```

It's evident that both functions replace `><"&` with their corresponding entity names, while the character `'\` remains unchanged. This implies that using `htmlentities` or `htmlspecialchars` alone may not fully protect against XSS vulnerabilities.

Now, let's examine the vulnerability by utilizing the following payload: `'onerror='alert("XSS")''` with the developer's code.
![XSS](https://github.com/X-Vector/XSS_Bypass/blob/master/htmlspecialchars%20-%20htmlentities/XSS.png?raw=true)

As demonstrated, it's feasible to bypass these functions by using single quotation marks in the code.
## How to Prevent XSS:
To effectively prevent XSS and ensure security, it's recommended to replace `'` with `&apos;`, remove `\`, and utilize both functions.
You can employ the following straightforward code to prevent XSS:
```php
function check($str)
{
  $str = preg_replace('#\'#','&apos;',$str);    // Replace [ ' ] with [ &apos; ]
  $str = preg_replace('#\\#','',$str);   // Remove [ / ]
  $str = htmlspecialchars($str); // or $str = htmlentities($str);
  return $str;
}
```
Alternatively, you can filter your input using:
```php
// Using htmlentities
$value = htmlentities($_GET['src'], ENT_QUOTES);

// Using htmlspecialchars
$value = htmlspecialchars($_GET['src'], ENT_QUOTES);
```


