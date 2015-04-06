As I understood, Julian is referring to situations like following and his suggestion for us is to focus on logical algebra and physical algebra when implementing common representation layer.

From Julians comments:
**'In the physical algebra the data stays in streaming format all the way through.**

**My point was that stream-to-relation and relation-to-stream occur in EVERY CQL query (and logical algebra) but do not necessarily occur in the physical algebra.'**

As I understand this type of scenarios can occur in our design too. Even though we refer to window operation as a stream-to-relation operation, what really happening in the actual operator implementation (based on CQL execution semantics) is transforming the original stream to a special type of stream called 'insert/delete stream'. It's the responsibility of down stream operators to handle this 'insert/delete' stream according to the CQL execution semantics.

But there are special cases like stream filtering shown below (from CQL paper [1]):

SELECT ISTREAM(*)
FROM PosSpeedStr [RANGE UNBOUNDED]
WHERE speed > 65

Physical plan for above query can be something like following:

PosSpeedStr Stream ----> (fileter: speed > 65) ----> Output Stream

So there aren't any window operators involved or relation-to-stream operators involved. 

Above query can also be written as:

SELECT RSTREAM(*)
FROM PosSpeedStr [NOW]
WHERE speed > 65 

But stream-to-relation and relation-to-stream operators can be absent in the execution plan for some queries.

[1] http://ilpubs.stanford.edu:8090/758/1/2003-67.pdf