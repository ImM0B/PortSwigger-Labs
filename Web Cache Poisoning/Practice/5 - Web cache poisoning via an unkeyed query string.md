Los propios parámetros de la url no están keyed, es decir, cambiar los parámetros no va a hacer que se haga una petición única al servidor, en su lugar se responderá con una respuesta cacheada de `/` 
Descubrimos que la cabecera Origin es un unkeyed input , inyectamos `"><script>alert(1)</script>` como parámetro en la url y quedará cacheado para el resto de usuarios cada 35 segundos.