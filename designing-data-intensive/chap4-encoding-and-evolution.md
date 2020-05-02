# Encoding and Evolution

When it comes to *evolvability* of system, we often needs to consider how an application would react to change in requirements i.e. schema / data model / encoding formats.

Whenever data format or schema changes, 2 keys things matter:

* *Backward compatibility*
  * Newer code can read data that was written by older code.
  
* *Forward compatibility*
  * Older code can read data that was written by newer code.
  
 **Note:** Backward compatibility is not hard to achieve, but forward compatibility can be trickier as it requires code to ignore additions made by newer versions of code.
 
 ## Formats for Encoding Data
 
 Programs interact with data in 2 ways:
 
 * In-Memory:
    * data structures (arrays, lists, heaps, trees, hash tables etc) for efficient access by CPU
 * Storage / Network
    * write data to a file (or) send it over a network.
    * for this, encoding is necessary to store as some sequence of bytes. (eg. JSON/XML/CSV etc)
 
 *Encoding* is translation from the in-memory representation to a byte sequence. (also known as serialization or marshalling)
 
 *Decoding* is translation from a byte sequence representation to in-memory. (parsing, deserialization, unmarshalling)
 
 ### Language Specific Formats
 
Languages often have built-in support for encoding in-memory objects into byte sequences. For example, Java has java.io.Serializable, Ruby
has Marshal, Python has pickle etc.

However, these are not good choice due to below reasons:

* Can't be read using another language code
* Poor support for versioning (i.e. backward/forward compatibility)
* Performance is not best.

### JSON, XML and Binary Variants

JSON, XML, and CSV are among popular textual based formats. However, they still have few issues to consider:

* Eg: In XML/CSV, you can't distinguish the number in string format, without actually looking at schema. 
JSON distinguishes numbers/strings, but not integers and floating-point. (also JSON can't hold number greater than 2^53, 
so you end up using string)

* JSON/XML have good unicode character support, but *no support for binary strings*.
  * Can get around using Base64 representation of binary data. 
  * This increases data size by about 33%

* CSV does not have any schema.
  * handle changes to columns manually.
  * if values have comma (,) or newline, you have to escape correctly.
 
#### Binary encodings of JSON/XML

Binary encodings might not make sense for small datasets, but for large datasets in terabytes, it can have big impact.

* JSON (MessagePack, BSON, BJSON, UBJSON, BISON, and Smile, to name a few)
* XML (WBXML and Fast Infoset, for example)

But these are not well adapated, compared to original text formats.

#### MessagePack (JSON binary variant)

Example record 

```
{
"userName": "Martin",
"favoriteNumber": 1337,
"interests": ["daydreaming", "hacking"]
}
```

[](MessagePack-image.png)

* The first byte, 0x83, indicates that what follows is an object (top four bits = 0x80)
with three fields (bottom four bits = 0x03).  

* The second byte, 0xa8, indicates that what follows is a string (top four bits =
0xa0) that is eight bytes long (bottom four bits = 0x08).

* The next eight bytes are the field name userName in ASCII. Since the length was
indicated previously, there’s no need for any marker to tell us where the string
ends (or any escaping).

* The next seven bytes encode the six-letter string value Martin with a prefix 0xa6,
and so on.


The binary encoding is *66 bytes* long, which is only a little less than the *81 bytes* taken by the textual JSON encoding 
(with whitespace removed).

### Thrift and Protocol Buffers



 
