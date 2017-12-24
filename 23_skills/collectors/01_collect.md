[TOC]

# 01 dot language

```reStructuredText
/* 
  Huffman Tree DOT graph.

  DOT Reference :  http://www.graphviz.org/doc/info/lang.html
                   http://en.wikipedia.org/wiki/DOT_language
  Timestamp     :  1514090580 
  Phrase        :  'IN THEIR CHRISTMAS MESSAGES THE PRIME MINISTER AND THE LABOUR LEADER PRAISE THOSE WHO HELP OTHERS.'

  Generated on http://huffman.ooz.ie/
*/

digraph G {
    edge [label=0];
    graph [ranksep=0];
    R [shape=record, label="{{R|8}|000}"];
    S [shape=record, label="{{S|9}|001}"];
    O [shape=record, label="{{O|4}|0100}"];
    L [shape=record, label="{{L|3}|01010}"];
    N [shape=record, label="{{N|3}|01011}"];
    A [shape=record, label="{{A|6}|0110}"];
    P [shape=record, label="{{P|3}|01110}"];
    C [shape=record, label="{{C|1}|0111100}"];
    B [shape=record, label="{{B|1}|0111101}"];
    D [shape=record, label="{{D|2}|011111}"];
    E [shape=record, label="{{E|13}|100}"];
    I [shape=record, label="{{I|7}|1010}"];
    T [shape=record, label="{{T|7}|1011}"];
    SPACE [shape=record, label="{{SPACE|15}|110}"];
    W [shape=record, label="{{W|1}|1110000}"];
    DOT [shape=record, label="{{DOT|1}|1110001}"];
    WDOT [label=2];
    G [shape=record, label="{{G|1}|1110010}"];
    U [shape=record, label="{{U|1}|1110011}"];
    GU [label=2];
    WDOTGU [label=4];
    M [shape=record, label="{{M|4}|11101}"];
    H [shape=record, label="{{H|8}|1111}"];
    98 -> 40 -> 17 -> R;
    23 -> 10 -> O;
    6 -> L;
    13 -> A;
    7 -> P;
    4 -> 2 -> C;
    58 -> 27 -> E;
    14 -> I;
    31 -> SPACE;
    16 -> 8 -> WDOTGU -> WDOT -> W;
    GU -> G;17 -> S [label=1];
    10 -> 6 -> N [label=1];
    2 -> B [label=1];
    40 -> 23 -> 13 -> 7 -> 4 -> D [label=1];
    27 -> 14 -> T [label=1];
    WDOT -> DOT [label=1];
    WDOTGU -> GU -> U [label=1];
    8 -> M [label=1];
    98 -> 58 -> 31 -> 16 -> H [label=1];
}
```



# 02 graphml

# 03 gephi: Open Graph viz platform