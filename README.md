Download Link: https://assignmentchef.com/product/solved-ece459-lab-2-channels-and-shared-memory
<br>
In this lab, you’ll be cracking JWT signatures. JWT stands for JSON Web Token, which is (from the <a href="https://en.wikipedia.org/wiki/JSON_Web_Token">Wikipedia pag</a><u>​ </u><a href="https://en.wikipedia.org/wiki/JSON_Web_Token">e</a>)​ “an Internet standard for creating data with optional signature​           and/or optional encryption whose <a href="https://en.wikipedia.org/wiki/Payload_(computing)">payloa</a>​            <a href="https://en.wikipedia.org/wiki/Payload_(computing)">d</a> ​ holds <a href="https://en.wikipedia.org/wiki/JSON">JSO</a>​ <a href="https://en.wikipedia.org/wiki/JSON">N</a> ​ that asserts some number of <a href="https://en.wikipedia.org/wiki/Claims-based_identity">claim</a>​      <a href="https://en.wikipedia.org/wiki/Claims-based_identity">s</a><a href="https://en.wikipedia.org/wiki/Claims-based_identity">”</a>​ . Basically, it’s a string consisting of a header, a payload and a signature. The three parts are separated by dots, and each of the three parts is encoded in base 64. Base 64 is an encoding that takes arbitrary binary data and converts it to a text format, and it’s typically used in URLs. Many web applications use JWTs for user authentication.​         Here’s is a typical JWT:




eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJ pYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI

The header is “eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ”​  , the payload is​

“eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9” and the signature​ is “gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI”.​

The signature is used to verify that the header and payload have not been tampered with. The header and the payload are combined with a value called the “secret”, and then hashed using HMAC-SHA256 to produce the signature. Specifically, the signature defined as:

HMAC-SHA256(base64UrlEncode(header) + “.” + base64UrlEncode(payload), secret)

The “secret” is what prevents malicious actors from generating fake JWT. Your task is to write a program that brute forces a JWT’s secret.

There are three positional command-line arguments to your program. The first is the token, the second is the maximum possible length of the secret, and the third is the alphabet (i.e. the set of characters that might be used in the secret).

You need to extract the signature portion from the token, and then use a brute-force approach to find the secret. You do this by exploring all possible values for the secret until one is found that produces the correct signature. The possible values for the secret are constrained by the key length and the alphabet. In a real-world situation, the problem is intractable because the alphabet is larger and the key length is longer (at least 32 characters). The examples used in this lab are chosen to have a short key length (four or five characters) and an alphabet consisting of the lowercase letters plus the digits 0 through 9.

Please don’t try to use this code on UW hardware to crack anything other than some test data for this exercise. The test data is sufficiently small that you can crack it in a few minutes; if it’s taking much longer than that, something is wrong.

We provide starter code that consists of a single-threaded implementation in main.rs. Instructions for running it can be found in the README file. Your job is to modify the code to improve its performance by using multiple threads.

In some cases, this is straightforward since the tasks are largely independent of each other. However, in this case things are more complex since the threads need to be able to communicate with each other. In particular, you want the threads to end once a solution has been found rather than exploring the entire solution space.

The two approaches you will be using are message passing and shared memory. The files message-passing.rs and shared-mem.rs are initially just copies of main.rs, and you’re expected to modify them to use message-passing and inter-thread communication respectively.

You’re free to implement things as you see fit. You may want to look at the Crossbeam crate, since it will do a lot of the work for  you.

For message passing, you are expected to use channels. There are multiple ways to do this, but we recommend that you start by looking at the unbounded channels provided by Crossbeam. Note that you should rely only on channels. You should not (and should not need to) make direct use of mutexes, atomics, or other shared state primitives. The channels do all this work for you.

By the same token (so to speak…), in the shared memory version you should not (and should not need to) use channels.


