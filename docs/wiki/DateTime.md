---
layout: default
title: Date and Time
---
#  Date and Time 

EcmaScript already supports a wide range of functions handling Date, see 
[https://developer.mozilla.org/en/Core\_JavaScript\_1.5\_Reference/Global\_Objects/Date][3] 

but those kind of setters/getters are not very convinient when working with 
DateTimes. an easy way to e.g. add/substract timeranges, calculate time-diffs,
etc. would be beneficial. 

problems to solve 

*   outputting / parsing datetime 
*   date calculations / manipulating 
*   json date extension (see crockford's json module)? 

including TZ conversion? naive datetime's - not knowing about TZ - are easier
to handle, though it seems JS has native aware datetime objects. utc &
localtime. 

##  Prior Art 

* RingoJS, minimal date extensions [http://github.com/ringo/ringojs/blob/master/modules/core/date.js][4]  
e.g.

        Date.diff
        Date.getTimespan
        Date.equals

*   datejs, clientside library. nifty & big. [http://code.google.com/p/datejs/][5] 

        // Get today’s date
        Date.today();
        // Add 5 days to today
        
        Date.today().add(5).days();
        // Get Friday of this week

        Date.friday();
        // Get March of this year
        Date.march();
        // Is today Friday?
        Date.today().is().friday();  // true|false

*   [dojo.date][6] and [dojo.date.locale][7] (dojo.date.stamp is obsoleted by ES5) 
    *   dojo.date provides convenience methods 
    *   dojo.date.locale provides format and parse functions which are data driven off the [Unicode CLDR project][8], fully localized client-side, downloading localizations only as needed. Support for hundreds of languages/variants. 

            >>> dojo.date.locale.format(myDate, {selector: “date”, formatLength: “long”}) // en-us
            “June 28, 2008″
            >>> dojo.date.locale.format(myDate, {selector: “date”, formatLength: “long”}) // zh-cn
            “2008年6月28日”

* [dojox.date][9] has experimental support for non-Gregorian calendars as well
as Unix- and PHP-style formatting.
* Matthew Eernisse implemented a [Date-like class][10] in JS to deal with the
Olson table client-side, as well as code to parse and preprocess the Olson
table. 
* ... 

 [1]: #column-one
 [2]: #searchInput
 [3]: https://developer.mozilla.org/en/Core_JavaScript_1.5_Reference/Global_Objects/Date "https://developer.mozilla.org/en/Core_JavaScript_1.5_Reference/Global_Objects/Date"
 [4]: http://github.com/ringo/ringojs/blob/master/modules/core/date.js "http://github.com/ringo/ringojs/blob/master/modules/core/date.js"
 [5]: http://code.google.com/p/datejs/ "http://code.google.com/p/datejs/"
 [6]: http://api.dojotoolkit.org/jsdoc/1.3/dojo.date "http://api.dojotoolkit.org/jsdoc/1.3/dojo.date"
 [7]: http://api.dojotoolkit.org/jsdoc/1.3/dojo.date.locale "http://api.dojotoolkit.org/jsdoc/1.3/dojo.date.locale"
 [8]: http://unicode.org/cldr "http://unicode.org/cldr"
 [9]: http://api.dojotoolkit.org/jsdoc/1.3/dojox.date "http://api.dojotoolkit.org/jsdoc/1.3/dojox.date"
 [10]: http://js.fleegix.org/plugins/date/date "http://js.fleegix.org/plugins/date/date"