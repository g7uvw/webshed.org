---
title: C++ Tips
permalink: wiki/C++_Tips/
layout: wiki
tags:
 - Code
 - C++
---

Some short routines and one-liners in C and C++ that may be useful
------------------------------------------------------------------

**Write a vector to a file**

` vector`<float>` test_vector;`  
` [write some data to vector]`  
` fstream file;`  
` file.open(`“`filename.bin`”`,ios::in|ios::out|ios::binary|ios::trunc);`  
` file.write((char*)&*(test_vector.begin()),test_vector.size() * sizeof(float);`  
` file.close();`
