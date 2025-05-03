**Usando DOM invader :**
[Client-side prototype pollution vulnerabilities | Web Security Academy](https://portswigger.net/web-security/prototype-pollution/client-side#finding-client-side-prototype-pollution-sources-manually)
[Testing for client-side prototype pollution - PortSwigger](https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution)

![[Pasted image 20250428114755.png]]

![[Pasted image 20250428114815.png]]


![[Pasted image 20250428115019.png]]

![[Pasted image 20250428115253.png]]

```javascript
Object.propotype

let myObject = {};
console.log(myObject.testproperty); // Output: 'DOM_INVADER_PP_POC'
```


Manual :

```
https://0a9e0053041c0868807d0dd200f500d2.web-security-academy.net/?__proto__[foo]=bar
```

![[Pasted image 20250428121726.png]]