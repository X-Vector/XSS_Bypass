# Bybass htmlentities & htmlspecialchars
- We Know That ` htmlentities() ,htmlspecialchars() ` it's Function to Prevent XSS
- When I Write This Code until I see how these functions are prevented XSS
```
<?php
$a = "!@#$%^&*()}{\'\"|?><`~}";
echo "htmlentities : ".htmlentities($a)."<br>";
echo "htmlspecialchars : ".htmlspecialchars($a);
 ?>
```
I Got This Result 
```
htmlentities : !@#$%^&amp;*()}{\'&quot;|?&gt;&lt;`~}
htmlspecialchars : !@#$%^&amp;*()}{\'&quot;|?&gt;&lt;`~}
```
We See That There Function Change There Char `><"&` to There Entity Name
and We See That This Char `'\` doesn't change these mean that if developer write this code By using htmlentities
```
$src = htmlentities($_GET['src']);
$see = "<img src='".$src."' width=30% height=30%> ";
echo $see;
```
or By Using htmlspecialchars 
```
$src = htmlspecialchars($_GET['src']);
$see = "<img src='".$src."' width=30% height=30%> ";
echo $see;
```
You Can Get XSS !!
Let's Try Using This Payload ` 'onerror='alert("XSS")'' ` with the Developer Code
XSS PHOTO HERE
as we have seen it is possible to skip this function if developer use single quotation in his code .

# How To Prevent XSS :
- To Prevent XSS and To Be Safe You Must replace `'` with `&apos;` and remove `\` 
- You Can Use This Simple Code To Prevent XSS in Your Site
```
function check($a)
{
  $a = preg_replace('#\'#','&apos;',$a);    // To remove [ ' ]
  $a = preg_replace('#\\#','',$a);   // To remove [ / ]
  $a = htmlspecialchars($a); // or $a = htmlentities($a);
  return $a;
}
```

