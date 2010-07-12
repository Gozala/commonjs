---
layout: wiki
title: CommonJS
---
Welcome to [CommonJS][3], a group with a goal of building up the JavaScript
ecosystem for web servers, desktop and command line apps and in the browser. 

This wiki is a starting point for collecting up ideas, links and any draft API
suggestions for the [CommonJS group][4]. Discussions occur on that mailing
list and [on IRC (#commonjs on freenode)][5]. 

## Contents

*   [1 Meta][7]
*   [2 Current Efforts][8]
*   [3 Future Efforts][9] 
    *   [3.1 Low-level APIs][10]
    *   [3.2 High-level APIs][11]
*   [4 Implementations][12]
*   [5 Tests][13]
*   [6 Further Reading][14]
*   [7 Wiki][15]

##  Meta 

*   [Introduction][16] 
*   [FAQ][17] 
*   [Process][18] 
*   [Target Platforms][19] 
*   [Coding Standards][20] 

##  Current Efforts 

This is a list of issues currently being discussed / standardized. They come
from the "Low level" department, as we need to have a solid basics prior to
building a tower. 

1.  [Uniform Baseline / Globals][21] (discussion) 
2.  [Modules][22] ([1.1.1][23]) 
    1.  `binary`: [Binary][24] Data Objects (byte arrays and/or strings) (proposals, discussion, early implementations) 
    2.  `encodings`: [Encodings][25] and character sets (proposals, discussion, early implementations) 
    3.  `io`: [I/O Streams][26] (proposals, discussion) 
    4.  `fs`, `fs-base`: [Filesystem][27] (proposals, discussion, early implementations) 
    5.  `system`: [System][28] Interface (stdin, stdout, stderr, &c) ([1.0][29], amendments proposed) 
    6.  `assert`, `test`: [Unit Testing][30] ([1.0][31], amendment proposals pending) 
    7.  `sockets`: [Socket I/O][32] TCP/IP sockets (early proposals) 
    8.  `event-queue`: [Reactor][33] Reactor/Event Queue (early proposals) 
    9.  `worker`: [Worker][34] Worker (concurrent shared nothing process/thread) (proposal) 
    10. `console`: [console][35] (proposal) 
3.  [Packages][36] ([1.0][37]) 
4.  [Web Server Gateway Interface (JSGI)][38] (proposals, discussion, early implementations) 
5.  [Promises][39] (proposal) 

*   [Pending Business / Calls for Action / Status Report][40] 

##  Future Efforts 

###  Low-level APIs 

This is the collection of APIs that we'd like to see behaving consistently
across JavaScript interpreters. 

*   [Language and Runtime Services][41] 
*   [Logging][42] 
*   [Relational database interface][43] 
*   ResultSets (collections of data maybe from RDBMS, maybe from other sources) 
*   [Concurrency][44] 
*   [String / ByteString I/O][45] 
*   [C unified API][46] to our Target Platforms 
*   [Subprocesses][47] (popen) 

###  High-level APIs 

This is the collection of APIs that implement common functionality on top of 
the low-level API. 

*   [HTTP client][48] APIs 
*   [Email][49] 
*   [Jabber (XMPP)][50] 
*   [Internationalization][51] 
*   [Promise Manager][52] 
*   [Command line processing][53] 

##  Implementations 

<table width="100%">
	<tbody>
		<tr>
			<th rowspan="2">
				<a href="/wiki/Implementations" title="Implementations">Implementations</a>
			</th>
			<th colspan="5"> Standards </th>
			<th colspan="11"> Proposals and standards in development </th>
		</tr>
		<tr>
			<th>
				<a href="/wiki/Modules/1.0" title="Modules/1.0">Modules/1.0</a>
			</th>
			<th>
				<a href="/wiki/Modules/1.1" title="Modules/1.1">Modules/1.1</a>
			</th>
			<th>
				<a href="/wiki/Modules/1.1.1" title="Modules/1.1.1">Modules/1.1.1</a>
			</th>
			<th>
				<a href="/wiki/Packages/1.0" title="Packages/1.0">Packages/1.0</a>
			</th>
			<th>
				<a href="/wiki/System/1.0" title="System/1.0">System/1.0</a>
			</th>
			<th>
				<a href="/wiki/Binary/B" title="Binary/B">Binary/B</a>
			</th>
			<th>
				<a href="/wiki/Console" title="Console">Console</a>
			</th>
			<th>
				<a href="/wiki/Encodings/A" title="Encodings/A">Encodings/A</a>
			</th>
			<th>
				<a href="/wiki/Filesystem/A" title="Filesystem/A">Filesystem/A</a>
			</th>
			<th>
				<a href="/wiki/Filesystem/A/0" title="Filesystem/A/0">Filesystem/A/0</a>
			</th>
			<th>
				<a href="/wiki/HTTP_Client/B" title="HTTP Client/B">HTTP Client/B</a>
			</th>
			<th>
				<a href="/wiki/Modules/Async/A" title="Modules/Async/A">Modules/Async/A</a>
			</th>
			<th>
				<a href="/wiki/Modules/Transport/C" title="Modules/Transport/C">Modules/Transport/C</a>
			</th>
			<th>
				<a href="/wiki/Modules/Transport/D" title="Modules/Transport/D">Modules/Transport/D</a>
			</th>
			<th>
				<a href="/wiki/Promises/B" title="Promises/B">Promises/B</a>
			</th>
			<th>
				<a href="/wiki/Unit_Testing/1.0" title="Unit Testing/1.0">Unit Testing/1.0</a>
			</th>
		</tr>
		<tr>
			<td><a href="/wiki/Implementations/CouchDB" title="Implementations/CouchDB">CouchDB</a></td>
			<td> yes </td>
			<td> </td>
			<td> yes </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
		</tr>
		<tr>
			<td><a href="/wiki/Implementations/Ejscript" title="Implementations/Ejscript">Ejscript</a></td>
			<td> </td>
			<td> 2.x </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
		</tr>
		<tr>
			<td> <a href="/wiki/Implementations/Flusspferd" title="Implementations/Flusspferd">Flusspferd</a> </td>
			<td> yes </td>
			<td> 0.9 </td>
			<td> </td>
			<td> </td>
			<td> yes </td>
			<td> yes </td>
			<td> </td>
			<td> yes </td>
			<td> </td>
			<td> yes </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> yes </td>
		</tr>
		<tr>
			<td> <a href="/wiki/Implementations/GPSEE" title="Implementations/GPSEE">GPSEE</a> </td>
			<td> yes </td>
			<td> yes </td>
			<td> </td>
			<td> </td>
			<td> yes </td>
			<td> yes </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> yes </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
		</tr>
		<tr>
			<td> <a href="/wiki/Implementations/Narwhal" title="Implementations/Narwhal">Narwhal</a> </td>
			<td> 0.1 </td>
			<td> 0.2 </td>
			<td> 0.2 </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> 0.5 </td>
			<td> 0.2 </td>
		</tr>
		<tr>
			<td> <a href="/wiki/Implementations/NarwhalJSC" title="Implementations/NarwhalJSC">Narwhal on JSC</a> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> 0.2 </td>
			<td> 0.2 </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
			<td> </td>
		</tr>
		<tr>
			<td> <a href="/wiki/Implementations/NarwhalNode" title="Implementations/NarwhalNode">Narwhal on Node</a> </td>
			<td>
</td><td>
</td><td>
</td><td>
</td><td> 0.5
</td><td> 0.5
</td><td>
</td><td>
</td><td> 0.5
</td><td> 0.5
</td><td>
</td><td>
</td><td>

</td><td>
</td><td>
</td><td>
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/NarwhalRhino" title="Implementations/NarwhalRhino">Narwhal on Rhino</a>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td> 0.2
</td><td> 0.2
</td><td>

</td><td>
</td><td> draft 4 in 0.1
</td><td> 8a45686
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/Persevere" title="Implementations/Persevere">Persevere</a>
</td><td> yes

</td><td>&nbsp;?
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td></tr>
<tr>

<td> <a href="/wiki/Implementations/RingoJS" title="Implementations/RingoJS">RingoJS</a>
</td><td> yes
</td><td> yes
</td><td>
</td><td>
</td><td> yes
</td><td> yes
</td><td>
</td><td>
</td><td> minus globbing

</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td> yes
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/Smart" title="Implementations/Smart">Smart Platform</a>
</td><td> yes
</td><td>

</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/SproutCore" title="Implementations/SproutCore">SproutCore 1.1/Tiki</a>

</td><td> yes
</td><td>&nbsp;?
</td><td>
</td><td>
</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>

</td><td> yes
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/Wakanda" title="Implementations/Wakanda">Wakanda</a>
</td><td> yes
</td><td> yes
</td><td> yes
</td><td>
</td><td> yes
</td><td>

</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td> yes
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/Yabble" title="Implementations/Yabble">Yabble</a>

</td><td> yes
</td><td>
</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td> yes
</td><td> yes

</td><td> yes
</td><td>
</td><td>
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/node.js" title="Implementations/node.js">node.js</a>
</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>

</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td> yes
</td></tr>
<tr>
<td> <a href="/wiki/Implementations/v8cgi" title="Implementations/v8cgi">v8cgi</a>
</td><td> yes

</td><td> yes
</td><td>
</td><td>
</td><td> yes
</td><td> yes
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>
</td><td>

</td><td> yes
</td></tr></tbody></table>


In development: 

*   mob is implementing SecurableModules in Ejscript [http://www.ejscript.org][82] 
*   pmuellr posted a sample loader for SecurableModules: [http://wiki.github.com/pmuellr/modjewel][83] 
*   atul varma has a SecurableModule implementation for Python/Spidermonkey: [http://hg.toolness.com/pydertron/raw-file/tip/docs.html][84] 
*   [Alexandre][85] implements several commonJS standards in Wakanda (SquirrelFish) [http://www.wakandasoft.com][86] 
*   [Franky Braem][87] tries to implement several commonJS standards in GLUEscript: [http://gluescript.sf.net][88]. 
*   Titanium is in the process of adding support for CommonJS. [http://www.appcelerator.com][89] 

##  Tests 

*   [CommonJS tests][90] compatible with [this test framework][91]. 

##  Further Reading 

*   [Existing APIs][92] 
*   [Infrastructure][93] 
*   [High Level Tools][94] 
*   [Random Links][95] 

##  Wiki 

*   [Dumps][96] 

 [1]: #column-one
 [2]: #searchInput
 [3]: http://commonjs.org "http://commonjs.org"
 [4]: http://groups.google.com/group/commonjs "http://groups.google.com/group/commonjs"
 [5]: http://log.serverjs.org/mochabot/join "http://log.serverjs.org/mochabot/join"
 [6]: javascript:toggleToc()
 [7]: #Meta
 [8]: #Current_Efforts
 [9]: #Future_Efforts
 [10]: #Low-level_APIs
 [11]: #High-level_APIs
 [12]: #Implementations
 [13]: #Tests
 [14]: #Further_Reading
 [15]: #Wiki
 [16]: /wiki/Introduction "Introduction"
 [17]: /wiki/FAQ "FAQ"
 [18]: /wiki/Process "Process"
 [19]: /wiki/Target_Platforms "Target Platforms"
 [20]: /wiki/Coding_Standards "Coding Standards"
 [21]: /wiki/Global "Global"
 [22]: /wiki/Modules "Modules"
 [23]: /wiki/Modules/1.1.1 "Modules/1.1.1"
 [24]: /wiki/Binary "Binary"
 [25]: /wiki/Encodings "Encodings"
 [26]: /wiki/IO "IO"
 [27]: /wiki/Filesystem "Filesystem"
 [28]: /wiki/System "System"
 [29]: /wiki/System/1.0 "System/1.0"
 [30]: /wiki/Unit_Testing "Unit Testing"
 [31]: /wiki/Unit_Testing/1.0 "Unit Testing/1.0"
 [32]: /wiki/Sockets "Sockets"
 [33]: /wiki/Reactor "Reactor"
 [34]: /wiki/Worker "Worker"
 [35]: /wiki/Console "Console"
 [36]: /wiki/Packages "Packages"
 [37]: /wiki/Packages/1.0 "Packages/1.0"
 [38]: /wiki/JSGI "JSGI"
 [39]: /wiki/Promises "Promises"
 [40]: /wiki/Status "Status"
 [41]: /wiki/Runtime_Services "Runtime Services"
 [42]: /wiki/Logging "Logging"
 [43]: /wiki/RDBMS "RDBMS"
 [44]: /wiki/Concurrency "Concurrency"
 [45]: /index.php?title=String_IO&action=edit&redlink=1 "String IO (page does not exist)"
 [46]: /wiki/C_API "C API"
 [47]: /wiki/Subprocess "Subprocess"
 [48]: /wiki/HTTP_Client "HTTP Client"
 [49]: /wiki/Email "Email"
 [50]: /wiki/XMPP "XMPP"
 [51]: /wiki/I18n "I18n"
 [52]: /wiki/Promise_Manager "Promise Manager"
 [53]: /wiki/Command_Line "Command Line"
 [54]: /wiki/Implementations "Implementations"
 [55]: /wiki/Modules/1.0 "Modules/1.0"
 [56]: /wiki/Modules/1.1 "Modules/1.1"
 [57]: /wiki/Binary/B "Binary/B"
 [58]: /wiki/Encodings/A "Encodings/A"
 [59]: /wiki/Filesystem/A "Filesystem/A"
 [60]: /wiki/Filesystem/A/0 "Filesystem/A/0"
 [61]: /wiki/HTTP_Client/B "HTTP Client/B"
 [62]: /wiki/Modules/Async/A "Modules/Async/A"
 [63]: /wiki/Modules/Transport/C "Modules/Transport/C"
 [64]: /wiki/Modules/Transport/D "Modules/Transport/D"
 [65]: /wiki/Promises/B "Promises/B"
 [66]: /wiki/Implementations/CouchDB "Implementations/CouchDB"
 [67]: /wiki/Implementations/Ejscript "Implementations/Ejscript"
 [68]: /wiki/Implementations/Flusspferd "Implementations/Flusspferd"
 [69]: /wiki/Implementations/GPSEE "Implementations/GPSEE"
 [70]: /wiki/Implementations/Narwhal "Implementations/Narwhal"
 [71]: /wiki/Implementations/NarwhalJSC "Implementations/NarwhalJSC"
 [72]: /wiki/Implementations/NarwhalNode "Implementations/NarwhalNode"
 [73]: /wiki/Implementations/NarwhalRhino "Implementations/NarwhalRhino"
 [74]: /wiki/Implementations/Persevere "Implementations/Persevere"
 [75]: /wiki/Implementations/RingoJS "Implementations/RingoJS"
 [76]: /wiki/Implementations/Smart "Implementations/Smart"
 [77]: /wiki/Implementations/SproutCore "Implementations/SproutCore"
 [78]: /wiki/Implementations/Wakanda "Implementations/Wakanda"
 [79]: /wiki/Implementations/Yabble "Implementations/Yabble"
 [80]: /wiki/Implementations/node.js "Implementations/node.js"
 [81]: /wiki/Implementations/v8cgi "Implementations/v8cgi"
 [82]: http://www.ejscript.org "http://www.ejscript.org"
 [83]: http://wiki.github.com/pmuellr/modjewel "http://wiki.github.com/pmuellr/modjewel"
 [84]: http://hg.toolness.com/pydertron/raw-file/tip/docs.html "http://hg.toolness.com/pydertron/raw-file/tip/docs.html"
 [85]: /wiki/User:Alexandre.Morgaut "User:Alexandre.Morgaut"
 [86]: http://www.wakandasoft.com "http://www.wakandasoft.com"
 [87]: /wiki/User:Fbraem "User:Fbraem"
 [88]: http://gluescript.sf.net "http://gluescript.sf.net"
 [89]: http://www.appcelerator.com "http://www.appcelerator.com"
 [90]: http://github.com/280north/narwhal/tree/master/tests/commonjs/ "http://github.com/280north/narwhal/tree/master/tests/commonjs/"
 [91]: http://github.com/280north/narwhal/tree/master/lib/test/ "http://github.com/280north/narwhal/tree/master/lib/test/"
 [92]: /wiki/Existing_APIs "Existing APIs"
 [93]: /wiki/Infrastructure "Infrastructure"
 [94]: /wiki/High_Level_Tools "High Level Tools"
 [95]: /wiki/Random_Links "Random Links"
 [96]: /wiki/Dumps "Dumps"