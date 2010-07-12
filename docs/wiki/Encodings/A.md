---
layout: wiki
title: Encodings/A
status: proposal
---
# Encodings/A

**STATUS: PROPOSAL** 

Implementations

[Flusspferd][5] 

For Streams, we need encodings support. There also should be a low-level API
available for this. 

## Contents[

[hide][6]]

*   [1 Specification][7] 
    *   [1.1 Encoding Names][8]
    *   [1.2 API][9] 
        *   [1.2.1 Simple methods][10]
        *   [1.2.2 Checking for available encodings][11]
        *   [1.2.3 Class: Transcoder][12]
*   [2 Usage examples][13] 
    *   [2.1 Stateful encodings][14]
*   [3 Implementation Recommendations][15]
*   [4 Relevant Discussions][16]
*   [5 Other][17]

#  Specification 

##  Encoding Names 

The encoding names should be among those supported by ICONV, which seem to be
a superset of [http://www.iana.org/assignments/character-sets][18]. 

The following encodings are required: 

*   US-ASCII 
*   UTF-8 
*   UTF-16 
*   ISO-8859-1 

Encoding names **must** be case **insensitive** 

##  API 

OK, so probably this should be a module: 

    :::js
	var enc = require('encodings')

###  Simple methods 

For convenience, there should be these easy methods for converting between
encodings: 

	string = enc.convertToString(sourceEncoding, byteStringOrArray) 

Converts a ByteString or a ByteArray to a Javascript string. 

	byteString = enc.convertFromString(targetEncoding, string) 

Converts a Javascript string to a ByteString. 

	byteString = enc.convert(sourceEncoding, targetEncoding, byteStringOrArray) 

Converts a ByteString or a ByteArray to a ByteString. 

###  Checking for available encodings 

	enc.supports(encodingName) 

Checks if encodingName is supported and return true if so, false otherwise. 

	enc.listEncodings([encodingCheckerFunction or regex]) 

encodingCheckerFunction takes the encoding name as a parameter and returns
true-ish if the encoding should be listed. Regexes should also be supported.
If the parameter is missing, returns all supported encodings. 

###  Class: Transcoder 

There also should be a class enc.Transcoder for general transcoding conversion
(between ByteStrings or ByteArrays). 

	[Constructor] Transcoder(from, to) 

Where from and to are the encoding names. 

	[Constant] sourceCharset 

String containing the (possibly normalised) source charset name. 

	[Constant] destinationCharset 

String containing the (possibly normalised) destination charset name. 

	[Method] push(byteStringOrArray[, outputByteArray]) 

Convert input from a ByteString or ByteArray. Those parts of byteStringOrArray
that could not be converted (for multi-byte encodings) are stored in a buffer.
If outputByteArray is passed, the results are *appended* to outputByteArray. 

If outputByteArray was passed, returns outputByteArray, otherwise returns _the
converted bytes as a ByteString_. 

_The result will also contain bytes accumulated in prior calls to
pushAccumulate._ 

	[Method] pushAccumulate(byteStringOrArray)

_Convert input from a ByteString or ByteArray into an internal buffer that
will be read out the next time push or close is called._ 

	[Method] close([outputByteArray]) 

Close the stream. Throws an exception if there was a conversion error
(specifically, a partial multibyte character). 

_Writes the remaining output bytes (including those that were accumulated in
pushAccumulate) into the here given outputByteArray (appended) or a new 
ByteString. If outputByteArray is given, it is returned, otherwise the 
ByteString is returned._ 

_Also adds initial shift state sequences if required by the encoding._ 

**TODO**: Which exception to throw on error? 

#  Usage examples 

Example: 

    Transcoder = require('encodings').Transcoder
    
    transcoder = new Transcoder('iso-8859-1', 'utf-32')
    transcoder.pushAccumulate(input) // input is a ByteString
    
    output = transcoder.close() // and output is a ByteString too

Another example: 

    transcoder = new Transcoder('utf-32', 'utf-8')
    
    output = new ByteArray()
    while (input = readSomeByteFromSomewhere()) {
    
            transcoder.push(input, output)
    }
    transcoder.close(output)
    
    // output is the complete conversion of all the input chunks concatenated now

(See [Encodings/OldClass][19] for another API.) 

##  Stateful encodings 

Some encodings can carry state across many characters - state which has to be
reset at the end of a conversion. This creates the need to *always* call 
transcoder.close(output) after converting. The simple methods take care of 
that already. An example of such an encoding would be [ISO-2022-JP][20], a 
Japanese encoding. This encoding is compatible with ASCII as long as it is in
the initial state. But there is an escape sequence (actually more than one: 
ESC ( J, ESC $ @ and ESC $ B) to get into Kana/Kanji mode, and another escape
sequence to get back into ASCII mode: ESC ( B. So a reader of ISO-2022-JP
content assumes that, initially, the encoding is in ASCII mode, and as soon 
as it sees an escape sequence, switches into another mode. Therefore, in
order to be able to concatenate files, ISO-2022-JP files should end with ESC
( B if they are in non-ASCII mode at the end. 

    // Te Su To - thanks to miwagawa
          katakana_string = "\u30c6\u30b9\u30c8",
    
          katakana_shift_jis = binary.ByteString([
            0x83,0x65,
            0x83,0x58,
    
            0x83,0x67
          ]),
    &nbsp;
          // Shift to JIS X 0208-1983
          iso2022_FROM_ascii = [ 0x1b,0x24,0x42 ],
    
          // Shift to ASCII
          iso2022_TO_ascii   = [ 0x1b,0x28,0x42 ],
    
    &nbsp;
          katakana_iso2022 = binary.ByteString(
            iso2022_FROM_ascii.concat([
              0x25,0x46,
    
              0x25,0x39,
              0x25,0x48,
            ], iso2022_TO_ascii)
    
          );

If you transcode as in the examples above, this should work. 

Flusspferd has some tests for these things in 
[http://github.com/ruediger/flusspferd/blob/master/test/js/encodings.t.js][21] 

#  Implementation Recommendations 

First of all, it is recommended to implement convertToString, 
convertFromString and convert with Transcoder. 

Secondly, you should make sure that initial shift state support is properly
implemented. When you're using iconv, you need to call iconv(cd, 0, 0, &ob,
&ol) in Transcoder.close(). An example of what an initial shift state is: In
the Japanese ISO-2022-JP encoding, the default state are ASCII bytes. However,
the state can be switched to Japanese with an escape sequence. To make sure
that at the end of the text, the state is ASCII again, iconv will emit another
escape sequence to switch back again. This is important if you want to
concatenate ISO-2022-JP texts, and an implementation of Transcoder that
doesn't properly emit these sequences is **broken**. 

#  Relevant Discussions 

*   [Proposal: Encodings API (independent from Streams)][22] 
*   [How to make Encodings][23] 
*   [Who's implementing the Encodings API?][24] 

#  Other 

[Flusspferd][25]'s implementation is documented [here][26] and [here][27]. 

[Category][28]: [Spec][29]

 [1]: /wiki/Encodings "Encodings"
 [2]: /index.php?title=Encodings/A/A&redirect=no "Encodings/A/A"
 [3]: #column-one
 [4]: #searchInput
 [5]: /wiki/Implementations/Flusspferd "Implementations/Flusspferd"
 [6]: javascript:toggleToc()
 [7]: #Specification
 [8]: #Encoding_Names
 [9]: #API
 [10]: #Simple_methods
 [11]: #Checking_for_available_encodings
 [12]: #Class:_Transcoder
 [13]: #Usage_examples
 [14]: #Stateful_encodings
 [15]: #Implementation_Recommendations
 [16]: #Relevant_Discussions
 [17]: #Other
 [18]: http://www.iana.org/assignments/character-sets "http://www.iana.org/assignments/character-sets"
 [19]: /wiki/Encodings/OldClass "Encodings/OldClass"
 [20]: http://en.wikipedia.org/wiki/ISO/IEC_2022 "http://en.wikipedia.org/wiki/ISO/IEC_2022"
 [21]: http://github.com/ruediger/flusspferd/blob/master/test/js/encodings.t.js "http://github.com/ruediger/flusspferd/blob/master/test/js/encodings.t.js"
 [22]: http://groups.google.com/group/commonjs/browse_thread/thread/6365b2a54615a134 "http://groups.google.com/group/commonjs/browse_thread/thread/6365b2a54615a134"
 [23]: http://groups.google.com/group/commonjs/browse_thread/thread/3faf4a067973a71a "http://groups.google.com/group/commonjs/browse_thread/thread/3faf4a067973a71a"
 [24]: http://groups.google.com/group/commonjs/browse_thread/thread/f9fcf27e1dd4a8b1 "http://groups.google.com/group/commonjs/browse_thread/thread/f9fcf27e1dd4a8b1"
 [25]: http://flusspferd.org "http://flusspferd.org"
 [26]: http://flusspferd.org/docs/js/commonjs%20core/encodings.html "http://flusspferd.org/docs/js/commonjs%20core/encodings.html"
 [27]: http://flusspferd.org/docs/js/commonjs%20core/encodings/transcoder.html "http://flusspferd.org/docs/js/commonjs%20core/encodings/transcoder.html"
 [28]: /wiki/Special:Categories "Special:Categories"
 [29]: /wiki/Category:Spec "Category:Spec"