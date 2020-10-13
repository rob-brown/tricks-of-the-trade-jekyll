---
layout: post
title:  "Abusing UTF-8"
author: Robert Brown
date:   2018-01-03 22:00:00 -0600
categories:
---
Recently, I learned of techniques to fingerprint documents by inserting invisible characters into the raw bytes. I decided to try it myself.

First, I picked two non-printable characters, one to respresent a logical 1 and the other a logical 0. Then, I convert each bit of the secret text to one of the non-printable characters. Finally, those bytes are injected into some mundane text. Extracting the secret text simply does the reverse.

My cursory tests show the modified text can be copied and pasted in many apps without any visual evidence of the lurking bytes. The encoding isn't optimal, but it does prove the concept.

Encoding secret messages is one reason of many why developers shouldn't make assumptions about UTF-8.

The full script can be found on [GitHub](https://gist.github.com/rob-brown/cdea88cbbdd6749a25db0cca0605498e).
