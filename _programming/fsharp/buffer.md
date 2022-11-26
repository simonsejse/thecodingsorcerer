---
layout: default
title: What is buffers?
nav_order: 3
tags: 
  - buffers
  - what are buffers
  - buffer
---

# Buffers
{: .py-1 }
{: .m-1 }
## Week 7
{: .p-1 }
{: .m-1 }
<hr/>
#### So what is a buffer?
{: .fs-5 }
{: .fw-700 }

You can think of a buffer as a temporary place to store your bytes. A kitchen analogy I got inspiration for from a fellow student is below. If it's confusing, skip it! (In that case, the hints in the XML code at the bottom of the announcement may be more useful).

Kitchen: Imagine that you have to roast (= Write()) 100 carrots from a sack (= a file / (fs:Filestream)) in the attic. But there is only room for 10 carrots on the pan (= 10 bytes in your memory). It would go horribly wrong if you poured 100 carrots directly onto the pan (=> some memory exception). So therefore you take out one bowl (= buffer). You go up to the attic and put 10 carrots in the bowl (= you read 10 bytes with fs.Read(buffer, 0, count), and note that the bytes are now in your buffer).

You take your buffer back to the kitchen and write to the file... Uh... I mean.. You take your carrots you have in the bowl back to the kitchen and fry the carrots (= you write the contents of the buffer to your output file ofs with .Write(buffer, 0, count)). Now that you have fried 10 carrots, you just have to make sure to fry the rest (you can coordinate this in readAndWriteBytes).

When do you know you have picked up the last carrots? When you have filled the bowl from the sack, but there are only 0 carrots left. Ok, that sounded a little silly. But fs.Read(...) returns the number of bytes it has read. When fs.Read(...) returns 0, you know that you have read an entire Filestream fs.
