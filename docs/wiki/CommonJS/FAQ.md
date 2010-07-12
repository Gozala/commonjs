---
layout: default
title: FAQ
---
# FAQ

### From CommonJS Spec Wiki



Jump to: [navigation][1], [search][2]

## Contents[

[hide][3]]

*   [1 Why use JavaScript on the server?][4]
*   [2 Why not Perl/PHP/Python/Ruby?][5]
*   [3 If sharing code is the objective, then why not compile language X to JavaScript to send to the browser?][6]
*   [4 Is there a bonus reason to use JavaScript on the server?][7]

###  Why use JavaScript on the server? 

As the scripting language of the web browser, JavaScript is and will be
familiar to many developers. The group of developers acquainted and fluent
with JavaScript likely will only increase over time. There is much developer
productivity to be gained by re-using that fluency. 

In addition, using the same language on both the client and server sides may
allow for some degree of code sharing. 

###  Why not Perl/PHP/Python/Ruby? 

While entirely worthy (to varying degrees) languages not in the browser will
always be familiar to a subset of web application developers, where JavaScript
fluency is more universal. When a higher order language is useful, JavaScript
is roughly comparable to the other higher order alternatives. (Naturally,
opinions will differ as to which is "best".) When developer fluency is
factored in, JavaScript becomes a pragmatic choice. 

Note that JavaScript inherits notions from some of the more interesting
object-oriented languages, including [Self][8], [Scheme][9], and Prototype -
all with a C/C++/Java-like syntax. 

###  If sharing code is the objective, then why not compile language X to JavaScript to send to the browser? 

Debugging compiled code is not an enjoyable experience. Often, the abstraction
layer between language X and JavaScript also has some holes then you end up
needing to plug with hand-crafted JS (and not just stock JS, but JS that is
designed to interface with that library). By using JavaScript all the way
across, there are no such leaks. 

Also, there can be some impedance mismatch between the prototype-based object
model in JavaScript and the class-based object models of other languages. 

###  Is there a bonus reason to use JavaScript on the server? 

Why yes, I'm glad you asked. JavaScript is powering increasingly complex
applications in the browser. This has caused browser vendors to work hard on
their JavaScript performance. Server side JavaScript has the potential to
significantly outperform other common dynamic languages because of this work. 


 [1]: #column-one
 [2]: #searchInput
 [3]: javascript:toggleToc()
 [4]: #Why_use_JavaScript_on_the_server.3F
 [5]: #Why_not_Perl.2FPHP.2FPython.2FRuby.3F
 [6]: #If_sharing_code_is_the_objective.2C_then_why_not_compile_language_X_to_JavaScript_to_send_to_the_browser.3F
 [7]: #Is_there_a_bonus_reason_to_use_JavaScript_on_the_server.3F
 [8]: http://en.wikipedia.org/wiki/Self_%28programming_language%29 "http://en.wikipedia.org/wiki/Self_(programming_language)"
 [9]: http://en.wikipedia.org/wiki/Scheme_%28programming_language%29 "http://en.wikipedia.org/wiki/Scheme_(programming_language)"