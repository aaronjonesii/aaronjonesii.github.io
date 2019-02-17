---
layout: post
title: Python Iteration of Attributes
image: https://user-images.githubusercontent.com/18272237/52909263-0d1a9b80-3254-11e9-9a97-3c818005ff0f.png
---
Python loop to iterate through an objects attributes while developing


## Iterate through an objects attributes w/their values
```python
for a,b in object.__dict__.items(): 
    print(f'{a} => {b}')
```




***
[Reference](https://www.saltycrane.com/blog/2008/09/how-iterate-over-instance-objects-data-attributes-python/)
