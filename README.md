Download Link: https://assignmentchef.com/product/solved-cs-300-homework-2-algorithm-for-data-compression
<br>
In this assignment you will implement an algorithm for<strong> data compression</strong>. The purpose of data compression is to take a file <strong>A</strong> and, within a reasonable amount of time, transform it into another file <strong>B</strong> in such a way that: (i) <strong>B</strong> is smaller than <strong>A</strong>, and (ii) it is possible to reconstruct <strong>A</strong> from <strong>B</strong>.  A program that converts <strong>A</strong> into <strong>B</strong> is called a <em>compressor</em>, and one that undoes this operation is called an <em>uncompressor</em>.  Popular programs such as WinZip perform this function, among others.  Compressing data enables us to store data more efficiently on our storage devices or transmit data faster using communication facilities since fewer bits are needed to represent the actual data.

Of course, a compressor cannot guarantee that the file <strong>B</strong> will always be smaller than <strong>A</strong>. It is easy to see why: if this were possible, then imagine what would happen if you just kept iterating the method, compressing the output of the compressor. In fact, with a little bit of thinking, you should be able to convince yourself that any compressor that compresses some files must also actually enlarge some files. (Understand why this should be the case!) Nevertheless, compressors tend to work pretty well on the kinds of files that are typically found on computers, and they are widely used in practice.

The Ziv-Lempel Algorithm

The algorithm that you will implement is a version of the <em>Ziv-Lempel data compression algorithm</em>, which is the basis for must popular compression programs such as <em>winzip</em>, <em>zip</em> or <em>gzip</em>. At first, you may find this algorithm a little difficult to understand, but in the end, the program that you will write to implement it should not be very long.

Ziv-Lempel is an example of an <em>adaptive data compression algorithm</em>. What this means is that the code that the algorithm uses to represent a particular sequence of bytes in the input file will be different for different input files, and may even be different if the same sequence appears in more than one place in the input file.

This is how the algorithm operates:  The Ziv-Lempel compression method maps strings of input characters into numeric codes. <em>To begin with, all characters that may occur in the text file (that is, the alphabet) are assigned a code</em>. For example, suppose the input file to be compressed has the string:

<strong>aaabbbbbbaabaaba </strong>

This string is composed of the characters ‘a’ and ‘b’. Assuming our alphabet of symbols is just {a, b}, initially ‘a’ is assigned the code 0 and ‘b’ the code l.  The mapping between character strings and their codes is stored in a <em>dictionary</em>.  Each dictionary entry has two fields:  <em>key</em> and <em>code</em>. The character string represented by <em>code</em> is stored in the field <em>key</em>. The initial dictionary for our example is given by the first two columns below (i.e., the shaded part with codes 0 and l):




<table width="0">

 <tbody>

  <tr>

   <td width="49">Code</td>

   <td width="49">0</td>

   <td width="49">1</td>

   <td width="49">2</td>

   <td width="49">3</td>

   <td width="49">4</td>

   <td width="49">5</td>

   <td width="49">6</td>

   <td width="49">7</td>

  </tr>

  <tr>

   <td width="49">Key</td>

   <td width="49">a</td>

   <td width="49">b</td>

   <td width="49">aa</td>

   <td width="49">aab</td>

   <td width="49">bb</td>

   <td width="49">bbb</td>

   <td width="49">bbba</td>

   <td width="49">aaba</td>

  </tr>

 </tbody>

</table>




<em> </em>

Beginning with the dictionary initialized as above, the Ziv-Lempel compressor repeatedly finds <strong>the longest prefix, <em>p,</em> of the unencoded part of the input file</strong> that is in the dictionary and outputs its code. If there is a next character <strong><em>c</em></strong> in the input file, then <strong><em>pc</em></strong> <em>(<strong>pc</strong></em> <em>is</em> the prefix string <strong><em>p</em></strong> followed by the character <strong><em>c</em></strong>) is assigned the next code and inserted into the dictionary. This strategy is called the Ziv-Lempel rule.

Let us try the Ziv-Lempel method on our example string. The longest prefix of the input that is in the initial dictionary is ‘a’. Its code, 0, is output, and the string ‘aa’ (<strong><em>p</em></strong> = “a” and <strong><em>c</em></strong> = ‘a’) is assigned the code 2 and entered into the dictionary.  “aa” is the longest prefix of the remaining string that is in the dictionary. Its code, 2, is output; the string “aab” (<strong><em>p</em></strong> = “aa”, <strong><em>c</em></strong> =’b’) is assigned the code 3 and entered into the dictionary. <em>Notice</em> <em>that</em> <em>even</em> <em>though</em> <em>‘aab’</em> <em>has</em> <em>the</em> <em>code</em> <em>3</em> <em>assigned</em> <em>to</em> it, <em>only</em> <em>the</em> <em>code</em> <em>2</em> <em>for</em> <em>‘aa’</em> <em>is</em> <em>output.</em> <em>The</em> <em>suffix</em> <em>‘b’</em> <em>will</em> <em>be</em> <em>part</em> <em>of</em> <em>the</em> <em>next</em> <em>code</em> <em>output.</em> <em>The</em> <em>reason for</em> <em>not</em> <em>outputting</em> <em>3</em> <em>is</em> <em>that</em> <em>the</em> <em>code</em> <em>table</em> <em>is</em> <em>not</em> <em>part</em> <em>of</em> <em>the</em> <em>compressed</em> <em>file.</em> <em>Instead,</em> <em>the</em> <em>code</em> <em>table</em> <em>has to be</em> <em>reconstructed</em> <em>during</em> <em>decompression</em> <em>using</em> <em>the</em> <em>compressed</em> <em>file.</em>  <em>This</em> <em>reconstruction</em> <em>is</em> <em>possible</em> <em>only</em> <em>if we</em> <em>adhere</em> <em>strictly</em> <em>to</em> <em>the</em> <em>Lempel-Ziv rule. </em>

Following the output of the code 2, the code for ‘b’ is output; “bb” is assigned the code 4 and entered into the code dictionary. Then, the code for “bb” is output, and “bbb” is entered into the table with code 5. Next, the code 5 is output, and “bbba” is entered with code 6. Then, the code 3 is output for “aab”, and “aaba” is entered into the dictionary with code 7. Finally, the code 7 is output for the remaining string ‘aaba’. Our sample string is thus encoded as the sequence of codes: 0 2 1 4 5 3 7.

The decompression algorithm proceeds as follows:  For decompression, we input the codes one at a time and replace them by the texts they denote. The code-to-text mapping can be reconstructed in the following way. The codes assigned for single character texts are entered into the dictionary at the initialization (just as we did for compression).  As before, the dictionary entries are code-text pairs. This time, however, the dictionary is searched for an entry with a given code (rather than with a given string). The first code in the compressed file must correspond to a single character (why?) and so may be replaced by the corresponding character (which is already in the dictionary!) For all other codes <strong><em>x</em></strong> in the compressed file, we have two cases to consider:

<ul>

 <li><strong>The code <em>x</em> is already in the dictionary:</strong> When <strong><em>x</em></strong> is in the dictionary, the corresponding text, <em>text(<strong>x</strong>),</em> to which it corresponds, is extracted from the dictionary and output. Also, from the working of the compressor, we know that if the code that precedes <strong><em>x</em></strong> in the compressed file is <strong><em>q</em></strong> and <em>text</em> <em>(<strong>q</strong>)</em> is the corresponding text, then the compressor would have created a new code for the text <em>text(<strong>q</strong>)</em> followed by the first character (that we will denote by fc<em>(<strong>x</strong>)</em>)<em>,</em> of <em>text(<strong>x</strong>) </em>(Understand why this is the case!) So, we enter the pair (next code, <em>text(<strong>q</strong>)fc(<strong>p</strong>))</em> into the dictionary.</li>

 <li><strong>The code <em>x</em> is NOT in the dictionary:</strong> This case arises only when the current text segment has the form <em>text(<strong>q</strong>)text(<strong>q</strong>)fc(<strong>q</strong>)</em> and <em>text(<strong>x</strong>)</em> = <em>text(<strong>q</strong>)fc(<strong>q</strong>) </em>(Understand why this is the case!) The corresponding compressed file segment is <strong><em>qx</em></strong><em>.</em> During compression, <em>text(<strong>q</strong>)fc(<strong>q</strong>)</em> <em>is</em> assigned the code <strong><em>x</em></strong><em>,</em> and the code <strong><em>x</em></strong> <em>is</em> output for the text <em>text(<strong>q</strong>)fc(<strong>q</strong>).</em> During decompression, after <strong><em>q</em></strong> is replaced by <em>text(<strong>q</strong>),</em> we encounter the code <strong><em>x</em></strong><em>.</em> However, there is no code-to-text mapping for <strong><em>x</em></strong> in our table. We are able to decode <strong><em>x</em></strong> using the fact that this situation arises only when the decompressed text segment is <em>text(<strong>q</strong>)text</em>(<strong><em>q</em></strong><em>)fc(<strong>q</strong>).</em> When we encounter a code <strong><em>x</em></strong> for which the code-to-text mapping is undefined, the code-to-text mapping for <strong><em>x</em></strong> is <em>text(<strong>q</strong>)fc(q),</em> where <strong><em>q</em></strong> <em>is</em> the code that precedes <strong><em>x</em></strong> in the file.</li>

</ul>

Let us try this decompression scheme on our earlier sample string <strong>aaabbbbbbaabaaba </strong>

which was compressed into the code sequence 0 2 1 4 5 3 7.

<ol>

 <li>To begin, we initialize the dictionary with the pairs (0, a) and (l, b), and obtain the first two entries in the dictionary above.</li>

 <li>The first code in the compressed file is 0. It is replaced by the text ‘a’.</li>

 <li>The next code, 2, is undefined. Since the previous code 0 has <em>text</em> <em>(</em>0<em>)</em> = ‘a’, <em>fc</em>(0)=’a’ then <em>text(</em>2<em>)</em> <em>=</em> <em>text(</em>0<em>)fc(</em>0<em>)</em> = ‘aa’. So, for code 2, ‘aa’ is output, and (2, ‘aa’) is entered into the dictionary.</li>

 <li>The next code, 1, is replaced by <em>text(</em>1<em>)</em> <em>=</em> ‘b’ and (3, <em>text</em>(2)<em>fc</em>(l) ) = (3, ‘aab’) is entered into the dictionary.</li>

 <li>The next code, 4, is not in the dictionary. The code preceding it is 1 and so <em>text</em>(4)= <em>text</em>(l)<em>fc</em>(l)= ‘bb’. The pair (4, ‘bb’) is entered into the dictionary, and ‘bb’ is output to the decompressed file.</li>

 <li>When the next code, 5, is encountered, (5, ‘bbb’) is entered into the dictionary and ‘bbb’ is output to the decompressed file.</li>

 <li>The next code is 3 which is already in the dictionary so text<em>(3)</em> <em>=</em> ‘aab’ is output to the decompressed file, and the pair (6, <em>text</em> <em>(</em>5<em>)fc</em>(3)) = (6, ‘bbba’) is entered into the dictionary.</li>

 <li>Finally, when the code 7 is encountered, (7, text(3)fc(3)) = (7, ‘aaba’) is entered into the dictionary and ‘aaba’ output.</li>

</ol>

<h2>Implementing the Ziv-Lempel Algorithm</h2>

We will use binary search trees as the basic dictionary data structure in the ZivLempel compression algorithm.  We will assume that our alphabet consists of 256 characters with values ranging from 0 to 255.  The first 256 integer codes 0 to 255, correspond to these characters respectively.  Since this is a very simple mapping, we do not have to store these; we will only store in the binary search tree, all strings with length 2 or more, along with their codes (which we know will start with 256), as we construct them during compression as discussed above.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

As you read new characters from the input file you create longer and longer strings which you search in the binary search tree. If you find any string (of length ≥ 2) in the tree but the next longer string is NOT in the tree, you do the following:

<ul>

 <li>You print out the code associated with the longest string you found to the output file.</li>

 <li>Then, unless you are at the end of the file, you create a string longer than the one you found using the next character read in (the character right after the string you found). You then associate the next available code (that you can keep track of with a counter) with the new string, inserting them to the tree.</li>

</ul>

Note once again that strings of length one will not be stored in the tree; you directly know what the codes for these are directly.

One implementation restriction is that you should be able to assign a maximum number codes. Let us assume for this homework that you will need at most 4096 code. Please note that codes 0 through 255 will be used for single character strings, so the first code that YOU will assign will be 256.  If during compression <strong><sup>           </sup></strong>, you find that you need more codes, you do not generate new strings but just use the available codes. This will give you less compression but otherwise it is not a problem.

The algorithm for uncompressing a file is actually simpler.  You need to keep an array (of size 4096) of strings.  This array is initialized for the first 256 character strings, so that, for instance, array position, 65 (which corresponds to ASCII symbol A) <strong>points</strong> to a string ″A″.  Starting with position 256, new character strings that are composed as described above for decompression are added to this table.  You can use a null pointer or a string of length 0 to detect that a string for a code is not yet composed.

You should convince yourself that these two data structures should be sufficient for you to implement the compression and decompression algorithms.

Your Task

You will complete the following for this homework:

You will design and implement a set of class for the data objects that you will store in the binary search tree. You should figure out what this looks like and what methods you need.  You can use the code for the binary search tree class provided in the textbook.

You will write two programs: The first one will perform compression.  To avoid a number of problems, we will make this program actually print out the sequence of codes as integers with <strong>one space</strong> after each integer.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> Thus, you will not actually compress but print the sequence of integer codes for the file to be compressed. The compression program will read the input from a file called <strong>compin</strong> and will produce a file called <strong>compout.</strong>

The second program will perform decompression. It should be able to read the file <strong>compout </strong>and produce the file <strong>decompout</strong> if it is functioning correctly. Obviously the contents of <strong>compin</strong> and <strong>decompout</strong> should be equal.

You will turn in your documented code for any classes, and for the compression and decompression programs.  Your code will be tested on the sample files we will use.

Your code should be submitted to SUCOURSE at the deadline given on the first page. You should follow the following steps:

<ul>

 <li>Name the two folders containing your source files as <em>XXXXX-NameLastnamecompress </em>and<em> XXXXX-NameLastname-decompress </em>where <em>XXXXX</em> is your student number. Put all the related files for the two programs into the respective directories. Make sure you do NOT use any Turkish characters in the folder name. <strong>You should remove any folders containing executables (Debug or Release), since they take up too much space.</strong></li>

 <li>Use the Winzip program to compress your folders to a compressed file named 0<em>5432-AliMehmetoglu.zip</em>. After you compress, please make sure it uncompresses properly and the uncompress should produce the two folders above.</li>

 <li>You then submit this compressed file in accordance with the deadlines above.</li>

</ul>

If the file you submitted is not labeled properly as above, if it does not uncompress, if the source code(s) do(es) not compile and if they do not work <em>for the input we will use</em>, as specified, we regretfully will have to give you 0 credits for the homework. So please make sure all these steps work properly.

<a href="#_ftnref1" name="_ftn1">[1]</a> You can also assume that you will never encounter a byte with value 0 in the input, which may interfere with normal I/O on some platforms. Just assume you have an English text file.

<a href="#_ftnref2" name="_ftn2">[2]</a> Actual compression will involve rather disgusting C++ code for dealing with details such as binary input output, bit packing, etc. which is really beyond the scope of this homework.