# Hack The Cause

This is a small write up for the CTF Hack the Cause, which can be found at http://hackthecause.info/ 

It's a small CTF, with only seven levels at the moment, but it seems like there will be more in the future, and the current levels are fun.

The challenges are beginner level, and are good for explaining certain topics, such as sqli. 
## Hidden In Plain Sight

This one is pretty straight forward, and is just what the title suggests. Look at the source, line #77
```html
<div style='display: none'><p>The Password Is 'good1Morty'</p></div>
```
Enter that, and done. Also, I appreciate the Ricl and Morty reference

## Client Side Problems
This challenge shows the problem with client side authentication. Using Firebug or another developer tool, you can see the javascript file
l2.js, which contains the line
```javascript
	$('#submit-pass').click(function() {
		if($('#pass-guess').val() == 'TisbutAFLESHwound') {
			flag_captured('three');
		} else {
			display_message('messages', 'Incorrect Password!', 1000);
```
And there you go, that's the password, enter and done.

## Daily Crypto

This challenge moves away from the hidden in plain sight type of problem, where you just have to sift through the text. When you click "hint"
it will give you a popup which says: "  Password:
      YmlyZFBlcnNvbg==". Now, it wouldn't really be that hard if that was the password, instead you have to figure out the relationship between that string and the password, which in this case ended
      up being base64 encoding.
Decoded it's "birdPerson"
Once again, a reference to Rick and Morty.

## SQL Injection 101
Just as the name suggests, this challenge is basic SQL injection. The hint given provides us with the query used:
```sql
SELECT * FROM users WHERE username="$user"
   AND password="$pass"
```
from this we can try to figure out how to obtain admin access. We want to be admin, so we set the user to admin, and now we need to get the password.
To get password we need get the right side of the query to be true, so we need to get something in there which is always true, like 1=1.
So, we insert 
```
' OR '1'='1'; -- 
```
For the password. The first ' ends the quote opened by the query string, and allows us to change the query. OR '1'='1' ensures it will always be true, and ; ends the query, -- makes anything else a comment.


## Input Modification
This is about using developer tools, you just have to modify the value for "No"

## Cross Site Script Kiddie

This challengeis some XSS. It asks you to get the page to alert("pwn3d"). The first thing I thought to do would be 
```javascript
<script>alert("pwn3d")</script>
```
But that didn't work. So I looked at the response the page gave. 
```html
>alert("pwn3d")</script>
```
That coupled with the hint "repetition is key" led me to try adding another <script> to the front, which worked.

## Multi Crypto
This is the final challenge, and it seems similar to #3. The hint this time is Password: YWJGYmhjU2JlSA==
Base64 decoding this seems to give something that isn't useful, but looking at the title we can tell there must be another step in decoding this.
CTFs tend to include the Vigenere, and rot(n). This string, once Base64 decoded is abFbhcSbeH.
I decided to check by going from easiest to hardest, and started with Rot(n). Using http://rumkin.com/tools/cipher/caesar.php I got all rotations for this string at once, and it ended up being the most common Rot 13.
The password being: noSoupForU 
