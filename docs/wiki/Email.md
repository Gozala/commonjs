---
layout: default
title: Email APIs
---
#  Email APIs 

Sending email is an extremely common server side activity and must be
supported. This means providing easy mechanisms for generating a MIME message
and sending email via an SMTP server (or possibly via a local command). 

##  Prior Art 

*   Microsoft [CDO.Message][1] object. Used in classic ASP, WSH, HTAs. 
*   Netscape Server [JavaScript 1.2 Sendmail API][2] 
*   PHP [PEAR Mail_Mime][3] : [Sample][4] 
*   PHP [Zend Mail][5]

 [1]: http://msdn.microsoft.com/en-us/library/aa488400%28EXCHG.65%29.aspx "http://msdn.microsoft.com/en-us/library/aa488400(EXCHG.65).aspx"
 [2]: http://research.nihonsoft.org/javascript/ServerReferenceJS12/sendmail.htm "http://research.nihonsoft.org/javascript/ServerReferenceJS12/sendmail.htm"
 [3]: http://pear.php.net/manual/en/package.mail.mail-mime.php "http://pear.php.net/manual/en/package.mail.mail-mime.php"
 [4]: http://pear.php.net/manual/en/package.mail.mail-mime.example.php "http://pear.php.net/manual/en/package.mail.mail-mime.example.php"
 [5]: http://framework.zend.com/manual/en/zend.mail.html "http://framework.zend.com/manual/en/zend.mail.html"