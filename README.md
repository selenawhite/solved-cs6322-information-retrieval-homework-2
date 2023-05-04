Download Link: https://assignmentchef.com/product/solved-cs6322-information-retrieval-homework-2
<br>
<strong>Problem  (300 points) </strong>

Index building

In this assignment you build two versions of the index for a simple statistical retrieval system and, each version of the index shall be in uncompressed form and compressed form. In the next assignment, you will build the retrieval system itself. For this assignment, index the Cranfield documents used in the last assignment:

A copy of the publicly available Cranfield collection is located on the UTD cs1, cs2, and csgrads1 machines at:

/people/cs/s/sanda/cs6322/Cranfield




You need to build:

<ul>

 <li><strong>uncompress</strong> (50 points) and</li>

 <li><strong>uncompress</strong> for the Cranfield collection (50 points).</li>

</ul>




Index_Version1.uncompress  considers the terms in the dictionary to be lemmas of words, Index_Version2.uncompress considers that the dictionary consists of stems of the tokens you have processed in Homework 1.




Do not store stop-words in any version of your index. Before building the dictionaries of any of the two versions of index, it is recommended to remove the stop words. You may use the stop-word list from:

/people/cs/s/sanda/cs6322/Cranfield/resourcesIR.




The dictionary in your 2<sup>nd</sup> version of the index should contain word stems obtained with the Porter stemmer. This index is referred to as Index_Version2. The code for the Porter stemmer is available in the same directory at:

/people/cs/s/sanda/cs6322/resourcesIR.




For every entry in the dictionary in both versions of the indexes, store:

<ul>

 <li>Document frequency (df): The number of documents that the term/stem occurs in. and</li>

 <li>The list of documents containing the term/stem.</li>

</ul>







For each document in the posting lists you are also required to store:

<ul>

 <li>the document ID;</li>

 <li>the Term/stem frequency (tf): The number of times that the term/stem occurs in that document;</li>

 <li>he frequency of the most frequent term or stem in that document (<em>max_tf</em>); (4)           the total number of word occurrences in the document (<em>doclen)</em>.</li>

</ul>

To be noted that the value of <em>doclen</em> includes the number of stop-words encountered in the respective documents.




Build each index using your preferred algorithm: Blocked sort-based Indexing (BSBI) or Single-pass in-memory indexing (SPIMI).




You also are required to build two compressed versions of your indexes:

<strong>Index_Version1.compressed</strong> and <strong>Index_Version2.compressed</strong>.

To do so, you shall:

(a)      compress the dictionaries of both versions of the index and  (b)     compress the inverted lists before storing them.




When compressing the inverted lists, you shall consider only the document ID, discarding tf, max_tf and doclen.




In Index_Version1.compressed you shall use for dictionary compression a <strong>blocked compression</strong> with k=4 (in this version of the index, the dictionary contains terms) and for the posting file you shall be using <strong>gamma encoding</strong> for the gaps between documentids. For this compressed index you shall receive 50 points.




In Index_Version2.compressed you shall use compression of the dictionary with <strong>frontcoding</strong> with the block size k=8 and for the posting files you shall use <strong>delta codes</strong> to encode the gaps between document-ids. For this compressed index you shall receive 50 points.




Delta codes are similar to the gamma codes: they represent a gap by a pair: (length, offset). First the number is represented in binary code. The length of the binary representation is encoded in gamma code, prior to removing the leading 1-bit. After generating the code of the length only, the leading 1-bit is removed and represented in gamma code.







<u>Example 1</u>: To write 5 in gamma and delta codes we perform the following operations:

<ol>

 <li>write 5 in binary as 101</li>

 <li>For the gamma code remove the leading 1-bit to obtain the offset: 01</li>

 <li>The length of the offset is 2:</li>

</ol>

<ul>

 <li>In unary the length is 110</li>

</ul>

<ol start="4">

 <li>The code of 5 in gamma is 11001 (or 110,01 – to represent [length, offset])</li>

 <li>For the delta code, the length of the offset is 3, because the leading 1-bit is removed afterwards. When writing the length=3 in gamma code it becomes:10,1</li>

 <li>for delta code, the leading 1-bit of the offset is removed now, generating an offset of 10</li>

 <li>The code of 5 in delta is 10101</li>

</ol>







<u>Example 2</u>: To write 9 in gamma and delta codes we perform the following operations:

<ul>

 <li>write 9 in binary as 1001</li>

 <li>for gamma code, remove the leading 1-bit to obtain the offset: 001</li>

 <li>The length of the offset is 3:</li>

 <li>In unary the length is 1110</li>

 <li>The code of 9 in gamma is 1110001 (or 1110,001 – to represent [length, offset])</li>

 <li>for the delta code, the length of the binary representation is 4</li>

 <li>The length is represented in gamma code: 11000</li>

 <li>The leading 1-bit of the binary representation is removed, generating the offset 001</li>

 <li>The code of 9 in delta is 11000001</li>

</ul>







<u>Example 3:</u> To write 1 in gamma and delta , we perform the following operations:

<ol>

 <li>write 1 in binary: 1</li>

 <li>For the gamma code, we remove 1, generating a length=0, which is still 0 in unary code</li>

 <li>in Gamma code, 1 becomes 0</li>

 <li>for the Delta code, the length of the binary representation for 1 is 1. Gamma code for 1 is 0</li>

 <li>The code for 1 in delta is 0</li>

</ol>
















More values for gamma and delta codes are given in the following table:







<table width="595">

 <tbody>

  <tr>

   <td width="31">N</td>

   <td width="60">Binary</td>

   <td colspan="7" width="226">Gamma code</td>

   <td colspan="2" width="38"> </td>

   <td colspan="8" width="240">Delta code</td>

  </tr>

  <tr>

   <td width="31">1</td>

   <td width="60">1</td>

   <td colspan="5" width="146">Len(-)=0; unary(0)=0;</td>

   <td colspan="2" width="80">Gamma(1)=0</td>

   <td colspan="2" width="38"> </td>

   <td colspan="6" width="160">Len(1)=1; Gamma(1)=0;</td>

   <td width="65">Delta(1)=0</td>

   <td width="15"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">2</td>

   <td rowspan="2" width="60">10</td>

   <td colspan="6" rowspan="2" width="156">Len(0)=1; unary(1)=10;</td>

   <td width="70">Gamma(2)</td>

   <td width="24">=100</td>

   <td rowspan="2" width="14"> </td>

   <td colspan="8" width="240">Len(10)=2; Gamma(2)=100;</td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="24"> </td>

   <td width="7"> </td>

   <td width="87">Delta(2)=1000</td>

   <td colspan="6" width="146"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">3</td>

   <td rowspan="2" width="60">11</td>

   <td colspan="6" rowspan="2" width="156">Len(1)=1; unary(1)=10;</td>

   <td width="70">Gamma(3)</td>

   <td width="24">=101</td>

   <td rowspan="2" width="14"> </td>

   <td colspan="8" width="240">Len(11)=2; Gamma(2)=100;</td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="24"> </td>

   <td width="7"> </td>

   <td width="87">Delta(3)=1001</td>

   <td colspan="6" width="146"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">4</td>

   <td rowspan="2" width="60">100</td>

   <td colspan="7" width="226">Len(00)=2; unary(2)=110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(100)=3; Gamma(3)=101;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td width="109">Gamma(4)=11000</td>

   <td colspan="5" width="109"> </td>

   <td width="7"> </td>

   <td colspan="2" width="94">Delta(4)=10100</td>

   <td colspan="5" width="139"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">5</td>

   <td rowspan="2" width="60">101</td>

   <td colspan="7" width="226">Len(01)=2; unary(2)=110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(101)=3; Gamma(3)=101;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td width="109">Gamma(5)=11001</td>

   <td colspan="5" width="109"> </td>

   <td width="7"> </td>

   <td colspan="2" width="94">Delta(5)=10101</td>

   <td colspan="5" width="139"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">6</td>

   <td rowspan="2" width="60">110</td>

   <td colspan="7" width="226">Len(10)=2; unary(2)=110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(110)=3; Gamma(3)=101;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td width="109">Gamma(6)=11010</td>

   <td colspan="5" width="109"> </td>

   <td width="7"> </td>

   <td colspan="2" width="94">Delta(6)=10110</td>

   <td colspan="5" width="139"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">7</td>

   <td rowspan="2" width="60">111</td>

   <td colspan="7" width="226">Len(11)=2; unary(2)=110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(111)=3; Gamma(3)=101;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td width="109">Gamma(7)=11011</td>

   <td colspan="5" width="109"> </td>

   <td width="7"> </td>

   <td colspan="2" width="94">Delta(7)=10111</td>

   <td colspan="5" width="139"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">8</td>

   <td rowspan="2" width="60">1000</td>

   <td colspan="7" width="226">Len(000)=3; unary(3)=1110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(1000)=4; Gamma(4)=11000;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td colspan="2" width="116">Gamma(8)=111000</td>

   <td colspan="4" width="102"> </td>

   <td width="7"> </td>

   <td colspan="3" width="116">Delta(8)=11000000</td>

   <td colspan="4" width="117"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">9</td>

   <td rowspan="2" width="60">1001</td>

   <td colspan="7" width="226">Len(001)=3; unary(3)=1110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(1001)=4; Gamma(4)=11000;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td colspan="2" width="116">Gamma(9)=111001</td>

   <td colspan="4" width="102"> </td>

   <td width="7"> </td>

   <td colspan="3" width="116">Delta(9)=11000001</td>

   <td colspan="4" width="117"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="31">10</td>

   <td rowspan="2" width="60">1010</td>

   <td colspan="7" width="226">Len(010)=3; unary(2)=1110;</td>

   <td colspan="2" rowspan="2" width="38"> </td>

   <td colspan="8" width="240">Len(1010)=4; Gamma(4)=11000;</td>

  </tr>

  <tr>

   <td width="7"> </td>

   <td colspan="3" width="131">Gamma(10)=1110010</td>

   <td colspan="3" width="88"> </td>

   <td width="7"> </td>

   <td colspan="4" width="123">Delta(10)=11000010</td>

   <td colspan="3" width="110"> </td>

  </tr>

  <tr>

   <td width="31"></td>

   <td width="60"></td>

   <td width="7"></td>

   <td width="109"></td>

   <td width="7"></td>

   <td width="15"></td>

   <td width="8"></td>

   <td width="10"></td>

   <td width="70"></td>

   <td width="24"></td>

   <td width="14"></td>

   <td width="7"></td>

   <td width="87"></td>

   <td width="7"></td>

   <td width="22"></td>

   <td width="7"></td>

   <td width="30"></td>

   <td width="65"></td>

   <td width="15"></td>

  </tr>

 </tbody>

</table>













Production-level IR systems build these compressed indices in about 5 minutes. If your program takes more than an hour, you are doing something wrong.




<em>Turn in your program, a written description of your program, and the following statistics, which will enable you to earn an <u>additional 100 points</u>: </em>

<em> </em>

<ul>

 <li>the elapsed time (“wall-clock time”) required to build any version of your index, 2 points per index)</li>

 <li>the size of the index Version 1 uncompressed (in bytes), 2 points</li>

 <li>the size of the index Version 2 uncompressed (in bytes), 2 points</li>

 <li>the size of the index Version 1 compressed (in bytes), 2 points</li>

 <li>the size of the index Version 2 compressed (in bytes), 2 points</li>

 <li>the number of postings in each version of the index, (2 points per index version) and</li>

 <li>the df, tf, and inverted list length (in bytes) for the terms:</li>

</ul>

“Reynolds”, “NASA”, “Prandtl”, “flow”, “pressure”, “boundary”, “shock”  (or stems that correspond to them) (2 points for each df, tf, 3 points for each inverted list length)

<ul>

 <li>the df, for “NASA” as well as the tf, the doclen and the max_tf, for the first 3 entries in its posting list. (10 points : 1 point for the df of “NASA”, 1 point for the tf in first document of the posting file of “NASA”, 1 point for the doclen of the fist document in the posting file and 1 point for the max-tf of the doclen and the max_tf, and so on)</li>

 <li>the dictionary term from index 1 with the largest df (2 points) and the dictionary term with the lowest df (2 points) – if more than one list them all!!!</li>

 <li>the stem from index 2 with the largest df (2 points) and the dictionary term with the lowest df (2 points) – if more than one list them all!!!</li>

 <li>the document with the largest max_tf in collection (4 points) and the document with the largest doclen in the collection (5 points)</li>

</ul>


