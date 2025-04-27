



### Fallo de Seguridad de /etc/passwd -> Pivoting a Usuario *HackerMan*

En sistemas basados en Linux, las contraseñas cifradas de los usuarios se almacenan en `/etc/shadow` por razones de seguridad. Sin embargo, si quieres modificar `/etc/passwd` para incluir una contraseña directamente en el campo de la contraseña, debes hacerlo manualmente. A continuación, te explico cómo hacerlo (aunque no es recomendable porque expones la contraseña a cualquier usuario que pueda leer `/etc/passwd`).

### Pasos para poner la contraseña en `/etc/passwd`:

1. **Generar el hash de la contraseña:** Las contraseñas en Linux deben estar cifradas usando un algoritmo como `MD5`, `SHA-256`, o `bcrypt`. Puedes generar el hash de una contraseña usando el comando `openssl` o `mkpasswd`.
    
    Por ejemplo:
    
    ```bash
    openssl passwd -6 'tu_contraseña'
    ```
    
    Esto generará un hash encriptado. Ejemplo de salida:
    
    ```
    $6$randomsalt$3G2F7ZtqqG.qN99tL0cUqeMJUPN2TRO8Ob3BqsGvIVhVYy8hZkB7z.TdHZRuLVH2vXEq2PxyFqJ47BAVdXmK91
    ```
    
    Este hash es lo que debes colocar en `/etc/passwd`.
    
2. **Editar `/etc/passwd`:** Abre el archivo `/etc/passwd` como root con un editor como `nano` o `vi`:
    
    ```bash
    sudo nano /etc/passwd
    ```
    
3. **Reemplazar el carácter `x`:** Busca la línea del usuario al que deseas modificar y reemplaza el carácter `x` en el segundo campo con el hash generado.
    
    Por ejemplo, cambia esto:
    
    ```
    usuario:x:1001:1001:Usuario Ejemplo:/home/usuario:/bin/bash
    ```
    
    A esto:
    
    ```
    usuario:$6$randomsalt$3G2F7ZtqqG.qN99tL0cUqeMJUPN2TRO8Ob3BqsGvIVhVYy8hZkB7z.TdHZRuLVH2vXEq2PxyFqJ47BAVdXmK91:1001:1001:Usuario Ejemplo:/home/usuario:/bin/bash
    ```
    
4. **Guardar los cambios:** Guarda el archivo y sal del editor.
    
5. **Probar el acceso:** Ahora deberías poder iniciar sesión con la contraseña que has establecido sin que el sistema recurra a `/etc/shadow`.

