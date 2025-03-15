La cookie de sesión tiene SameSite Strict, por lo que no será enviada en ninguna petición que no sea entre el propio dominio.

```
/resources/js/commentConfirmationRedirect.js
```

```javascript
redirectOnConfirmation = (blogPath) => {
    setTimeout(() => {
        const url = new URL(window.location);
        const postId = url.searchParams.get("postId");
        window.location = blogPath + '/' + postId;
    }, 3000);
}
```