# Primeros pasos
## Main Page
![[Pasted image 20250126004147.png]]
Simplemente La página de inicio.
## About
![[Pasted image 20250126011218.png]]
Nos muestra información de que son los CTF o Capture the Flag.
## Login Page
![[Pasted image 20250126004355.png]]
Parece que tenemos que estar registrados previamente para poder iniciar sesión.
## Register Page
![[Pasted image 20250126004415.png]]
Parece ser que tenemos que tener un ***RecieptId*** para poder registrarnos. Si tratamos de dar clic en la palabra destacada de ***"Don't have reciept id? Register [[here]]"*** visualizamos que somos redirigidos a la misma pagina lo unico que se nos ha añadido un [ancla](https://developer.mozilla.org/es/docs/Web/HTML/Element/a#href) . Este nos muestra la siguiente frase:
![[Pasted image 20250126005429.png]]
```markdown
#Really?You?are?super?hacker?!#Try#Something
```
Nos intenta molestar y nos da la mini pista de Try Something. Aquí empieza el primer reto. ¿Seremos capaces de conseguir registrarnos?
- Revisamos el código HTML para tratar de entender cómo funciona el propio formulario de registro. Y nos encontramos lo siguiente *(Se ha quitado la identación para poder visualizar mejor el contenido)*:
```html
<form method="POST" action="register.php">
<!-- Reciept ID -->
<div class="form-group">
<input type="text" class="form-control" name="reciept_id" id="reciept_id" value="" placeholder="RecieptId(ex. AAAA-BBBB-CCCC-DDDD-123)">
<small id="reciept_id_help" class="form-text text-muted">Don't have reciept id? Register <a target="_blank" href="#Really?You?are?super?hacker?!#Try#Something">here</a></small>
</div>
<!-- Team Name -->
<div class="form-group">
<input type="text" class="form-control" name="team_name" id="team_name" value="" placeholder="Team name">
</div>
<!-- Password -->
<div class="form-group">
<input type="password" class="form-control" name="password" id="password" value="" placeholder="New Password">
<small id="passHelp" class="form-text text-muted">Make sure nobody's behind you</small>
</div>
<!-- Checkbox: Solemn Oath -->
<div class="custom-control custom-checkbox my-4">
<input type="checkbox" class="custom-control-input" name="solemnly_swear" id="solemnly-swear">
<label class="custom-control-label" for="solemnly-swear">I solemnly swear that I am up to no good.</label>
</div>
<!-- Submit Button -->
<button type="submit" class="btn btn-outline-danger btn-shadow px-3 my-2 ml-0 ml-sm-1 text-left typewriter">
<h4>Register</h4>
</button>
</form>
```
Una vez analizado vemos que no encontramos nada fuera de lo normal, pero nos da una pequeña pista dandonos un ejemplo de ***RecieptId*** `RecieptId(ex. AAAA-BBBB-CCCC-DDDD-123)`, podemos tratar de mandar el mismo ejemplo para ver si funciona.
![[Pasted image 20250126010134.png]]
![[Pasted image 20250126010148.png]]
Nos redirige a login.php directamente asi que podemos tratar de probar si hemos podido registrar correctamente el usuario o bien podemos revisar en la pestaña de Hackerboard donde muestra participantes del CTF.

![[Pasted image 20250126010323.png]]
Efectivamente vemos que se ha generado el usuario ***j0rd1s3rr4n0***. Tratamos de entrar con las credenciales que hemos generado:
![[Pasted image 20250126010514.png]]
Nos muestra la página de instrucciones del CTF donde vemos el siguiente texto:
![[INSTRUCTIONS]]

Bien aquí vemos que ya se nos comentan que hay dos tipos de **flags** diferentes las que empiezan por ``CTF{algo_chulo}`` o las que empiezan por ``flag{algo_chulo}``. Tambien nos plantean que hay [6 flags ocultas](TramuntHackCTF#Hidden Optional Flags) en las páginas. 
# Quests
## Challenges
### Forensics
Nos plantean 3 retos de Forensia digital algunos basados en hechos reales.
- [[Forensic Case 1]]
- [[Forensic Case 2]]
- [[Forensic Case 3]]
### Reversing
Nos plantean 3 retos de REVESING donde tenemos que hacer uso o extraer la contraseña para visualizar y/o usar la flag.
- [[ExtractMyPassword Level 1]]
- [[ExtractMyPassword Level 2]]
- [[ExtractMyPassword Level 3]]
## Machines
Son dos máquinas y/o Infraestructuras a las que tendremos que auditar, conseguir acceso a ellas y conseguir dos objetivos o flags situadas en los archivos ***user.txt*** y ***root.txt***. Dispondremos de 3 pistas o Hints que no restaran puntos al resolver la máquina pero si que se recomienda no usarlas para una experiencia más real de auditoria y para una competición limpia.
- [[VulnWeb]]
- [[TramuntHackCTF/HackerManLand/HackerManLand]]
*Ejemplo:*
![[Pasted image 20250126012422.png]]
## Hidden Optional Flags
Estos son los 6 retos opcionales ocultos en la web de TramuntHackCTF management.
- [[The 6 Hidden Flags]]