---
layout: default
title: What is buffers?
parent: FSharp
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

<hr/>

I kan tænke på en buffer som en midlertidigt sted at opbevare jeres bytes. En køkken-analogi jeg fik inspiration til fra en medstuderende står nedenfor. Hvis den er forvirrende, så spring den over! (I så fald kan det være hintsene i XML koden nederst i announcement er mere brugbar).

Køkken: Forestil jer at I skal stege (=  Write()) 100 gulerødder fra en sæk  (= en fil / (fs:Filestream)) på loftet. Men der er kun plads til 10 gulerødder på panden (= 10 bytes i jeres memory). Det vil gå grueligt galt hvis I hældte 100 gulerødder direkte på panden (=> en eller anden memory exception). Så derfor tager I én skål (= buffer) frem. I går op på loftet og ligger 10 gulerødder i skålen (= i læser 10 bytes med fs.Read(buffer, 0, count), og bemærk at de bytes nu er i jeres buffer).

I tager jeres buffer med tilbage til køkkenet og skriver til filen... Øh... Jeg mener.. I tager jeres gulerødder I har i skålen tilbage til køkkenet, og steger gulerødderne (= I skriver indeholdet i buffer til jeres output fil ofs med .Write (buffer, 0, count)). Nu hvor I har stegt 10 gulerødder så skal I så bare sørge for at stege resten (det kan i koordinere i readAndWriteBytes).

Hvornår ved I så at I har hentet de sidste gulerødder? Når I har fyldt skålen op fra sækken, men der kun er 0 gulerødder tilbage. Ok, det lød lidt fjollet. Men fs.Read(...) returnerer de antal bytes den har læst. Når fs.Read(...) returnerer 0, så ved I at I har læst en hel Filestream fs. 
