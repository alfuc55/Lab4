# Lab4
Plot Juggler y URDF
# Introduccion

# Desarrollo 
## URDF

## Plot Juggler 
Con el objetivo de comprobar los resultados del controlador desarrollado e implementado en la práctica anterior, se deberá usar la herramienta plot juggler para graficar las variables y comprobar que el error es muy cercano a cero y asi verificar la precisión del controlador. 
Una vez que plot juggler es instalado en la misma maquina Linux junto con ROS, se procede a levantar el Core de ROS y luego a abrir el simulador turtlesim, después se ejecuta el nodo de ROS programado en Python de la practica anterior. Para que la herramienta de graficación plot juggler pueda leer los datos se conecta a la red de ros como un subscrber y se puede subscribir a todos los nodos disponibles para leer las variables en tiempo real y crear las gráficas.  En cuanto a las variables del sistema que corresponden a velocidad y posición ya están siendo publicadas a la red de ros en el nodo programado en la práctica anterior, por lo que ya es posible graficarlas mediante plot juggler, sin embargo el error no está siendo publicado a la red de ROS y por lo tanto no hay forma de que plot juggler tenga acceso a la variable, por lo que para que sea posible su graficación es necesario crear un tópico de ROS donde se Publique el mensaje y pueda ser leído plotjuggler mediante la red de ROS. 
Para publicar el error en un nuevo tópico de ROS son agregadas las siguientes líneas al código del nodo implementado en la práctica anterior
Incluir el tipo de mensaje FLOAT64 mediante la librería std_msgs.msg
        from std_msgs.msg import Float64	

después, dentro de la función inicial llamada init crear el publicador, también es necesario darle un valor mayor al buffer para que almacene mas datos y la grafica sea mas extensa
        self.errorPub = rospy.Publisher('/turtle1/error_topic',Float64, queue_size=1000)

por último, después de la línea que publica el error en la consola: 
        rospy.loginfo(float(error_x))
agregar la línea: 
            self.errorPub.publish(error_x)
que publicara el error en x en la red de ROS. 
Una vez que el código ha sido modificado al ejecutarlo ahora se debe estar creando otro topic que manda el error en x de la tortuga.
El ultimo paso es abrir plotjuggler mediante el comando: 
		Rosrun plotjuggler plotjuggler. 
Para la graficación de variables se deberá iniciar el graficador mediante el botón start, esto desplegara la ventana con los tópicos disponibles, aquí se seleccionan los valores que se desean graficar, en nuestro caso el valor mas importante para verificar el funcionamiento del controlador el valor del error, y también la posición en x, theta y la velocidad lineal en x. Se seleccionarán estos tópicos en la ventana y se dará en OK.
Ahora para una mejor presentación de los resultados se dividirá verticalmente la ventana y se arrastraran las variables deseadas para que se grafiquen. 
Ahora al ingresar las coordenadas deseadas para la tortuga plotjuggler deberá graficar en tiempo real las variables asignadas. 



