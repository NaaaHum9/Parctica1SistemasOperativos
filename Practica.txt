#!/bin/bash

#creacion de archivos para la opcion 3
touch arch1 |echo "#! /bin/bash">arch1; echo "chmod +r arch1">>arch1; chmod +x ./arch1; ./arch1
touch arch2 |echo "#! /bin/bash">arch2; echo "chmod +x arch2">>arch2; chmod +x ./arch2; ./arch2
touch arch3 |echo "#! /bin/bash">arch3; echo "ls">>arch3; chmod +x ./arch3

#Menu

function menu(){

opcion="$1"

case "$opcion" in
1)
echo "Ejericio 1"
echo "1) Imprime las ultimas 15 lineas de log en orden inverso"
cd /var/log
sudo chmod +r syslog #concede permisos de lectura como un super usuario
cd ~
tail -n 15 /var/log/syslog |nl | tac >syslog.txt | >/dev/null
;;

2)
echo "Ejercicio 2"
a=10
echo "a = $a"
b=$((2*10))
echo "b = $b"
c=$(((a+b)/2))
echo "c = $c"
d=$(((2+3)*10))
echo "d = $d"
div=$(("$c/2" | bc -l))
echo "div = $div"
res=$(("$b%$c"| bc -l))
echo "res = $res"
;;

3)
echo "Ejercicio 3"
echo "3) Validacion de 3 archivos"
#archivo principal
#permisos de lectura para el primer archivo
if [ ! -r "arch1" ]; then
echo "El archivo 1 no tiene permisos de lectura"
else
echo "El archivo 1 tiene permisos de lectura"
fi

#existencia y permisos de ejecucion
if [ ! -f "arch2" ]; then
echo "El archivo 2 no existe"
else
echo "El archivo 2 existe"
fi

if [ ! -x "arch2" ]; then
echo "El archivo 2 no tiene permisos de ejecucion"
else
echo "El archivo 2 si tiene permisos de ejecucion"
fi

#mostrar propietarios
usuario=$(stat --format=%U arch3)
echo "el usuario del archivo es: $usuario"

#archivo mas antiguo
archAntiguo=$(ls -t "arch1" "arch2" "arch3" |tail -1)
echo "El mas antiguo es $archAntiguo"
;;

4)
echo "Ejercicio 4"
env | while read -r variable; do
if [ -n "$variable" ]; then
echo "$variable Con contenido"
else
echo "$variable Sin contenido"
fi
done
if [ -z "$HOSTNAME" -o "HOTSNAME" = "(none)" ]; then
echo "La variable de entorno no tiene contenido"
fi
;;

5)
echo "Ejercicio 5"
echo "5) Argumentos de ejecucion"
# $# verifica si hay al menos un argumento
if [ $# -gt 0 ]; then
 echo "Numero de argumentos: $#"
 echo "Valores de los argumentos: "
 for argu in "$@"; do
  echo "$argu"
 done
else
 echo "Sin argumentos"
fi
;;

7)
echo "Ejercicio 7"
read -p "Ingrese el directorio para la busqueda: " busqueda
read -p "Ingrese el patron: " patron
archivo="resumen.txt" > "$archivo"
grep -r "$patron" "$busqueda" > "$archivo"
echo "Búsqueda completada"
;;

salir)
echo "Salio del programa Practica 1"
;;

*)
echo "Salio del programa pratica 1"
esac
}

while true; do
echo "opciones: "
echo "Ejericio 1"
echo "Ejercicio 2"
echo "Ejercicio 3"
echo "Ejercicio 4"
echo "Ejercicio 5"
echo "Ejercicio 7"
echo "salir"
read opcion
menu "$opcion"
[ "$opcion" = "salir" ] && break
done
