# Lab4
URFD y Plot Juggler 

Reporte del Laboratorio #4 de LRT4012

165096 Diego de Jesús Gutiérrez Reyes

167464 Aldo Fuentes Cruz

# Introduccion

## URDF
El Formato Unificado de Descripción de Robot (Unified Robot Description Format), se trata de un formato XML empleada en la representación del modelo de un robot, es usualmente empleados en herramientas de sistemas operativos de robots, como es el caso de ROS o Gazebo. El modelo del robot se púede crear gracias a que se incluyen enlaces y movimientos de articulaciones.

Dentro del código XML, cada elemento "Link" describe un cuerpo rígido en cuanto a inercia, geometría visual y forma de colisión se refiere, por su parte los elementos "Joint" definen las propiedades cinemáticas y dinámicas, así como los limites de cada uno de los eslabones.

Para cada Link se requiere de la configuración de 3 bloques principales: inertial, visual y colision:

Para inertial se emplean los siguientes comandos:
- origin: que define la posición del sistema de referencia inercial.
- mass: que define la masa total del cuerpo.
- inertia: que crea la matriz de inercia medida en el centro de la masa del cuerpo, se define en 6 elementos.

Para la sección visual, se tiene:
- origin: que proporciona el origen del sistema de referencia visual.
- geometry: que define la forma visual del cuerpo, permitiendo la definición de la posición mediante "X Y Z" y de la orientación mediante "R P Y".
- material: se le asigna un color en formato RGB.

Finalemnte, para el bloque colision, se tienen los comandos:
- origin: que es el origenm del sistema de referencia visual.
- geometry: que da el aspecto de la forma visual del cuerpo, pudiedo ser cuadrado, esférico, cilíndrico, entre otros.

Por su parte, para las Joints, primero es importante definir el tipo de articulación, estas peuden ser:
- Revolute: que poseen un grado de libertad en torno a un eje.
- Fixed: articulación fija que no posee grados de libertad.
- Continuos: articulación similar a revolute, pero sin límites de rango de movimiento.
- Prismatic: posee un grado de libertad de desplazamiento linear sobre un eje.
- Planar: articulación con dos grados de libertad que permite el movimiento en un plano perpendicular.
- Floating: movimiento totalmente libre que incluye 6 rangos de libertad.

Finalemnte, algunos de los comandos empleados en las articulaciones son:
- origin: que indica la transformada desde el eslabón del padre con el eslabón del hijo.
- parent: posee el nombre del eslabón padre al que está unido el joint.
- child: posee el nombre del eslabón hijo al que está unido el joint.
- axis: representa el eje de giro o traslación de la articulación.
- dynamic: propiedades físicas de la articulación, permite especificar los valores de fricción y amortiguación.

## Plot Juggler 

Se trata de una herramienta rápida, intuiiva y extensible de visualización, es perfecto para la visualización de registros, datos fuera de línea y tiempo real, y posee un uso múltiples campos de investigación y análisis, destacando sobretodo en rbótica, concretamente en áreas de vehículos y drones; ciencia de datos y automatización.

Posee acceso a múltiples fuentes de datos, como lo son ROS y ROS2, PX4 o mQTT, el uso de la herramienta en este laboratorio se orienta exclusivamente en ROS, para el análisis de distintos robots. El menú de Plot Jagger se muestra en la sigueitne figura, adicionalemnte se muestra la graficación de ciertos parámetros.

![image](https://github.com/alfuc55/Lab4/assets/132300202/a70dcad2-824d-43c5-8585-344d4f7367ae)


# Desarrollo 
## URDF

Para la creación de un robot caertesiano se emplea el sitio web "My Model Robot", el cual permite, mediante código XML, la generación de un modelo de un robot, al configurar Joints y Links, el robot propuesto se trata de un brazo robótico con movimiento en los 3 ejes, para ello se crea una base y 3 eslabones, cada uno permitiendo el movimiento en uno de los ejes X, Y y Z, el código del robot es el siguiente:



<robot name="LAB4">

	<!-- * * * Link Definitions * * * -->

 	<link name="base">
		<visual>
		    <origin xyz="0 0 0" rpy="0 0 0"/>
			<geometry>
				<cylinder radius="0.2" length="0.4"/>
			</geometry>
			<material name="black1">
	       		<color rgba="0.2 0.2 0.2 1.0"/>
	     	</material>
		</visual>	
	</link>



<link name="brazo1">
		<visual>
		    <origin xyz="0 0.0 0.35" rpy="0 0 0"/>
			<geometry>
				<box size="0.1 0.1 0.8"/>
			</geometry>
			<material name="Cyan2">
	       		<color rgba="0 0.7 0.7 1.0"/>
	     	</material>
		</visual>	
	</link>

<link name="brazo2">
		<visual>
		    <origin xyz="0.2 0.0 0.0" rpy="0 1.57 0"/>
			<geometry>
				<box size="0.1 0.1 0.4"/>
			</geometry>
			<material name="Cyan2">
	       		<color rgba="0 0.7 0.7 1.0"/>
	     	</material>
		</visual>	
	</link>

<link name="brazo3">
		<visual>
		    <origin xyz="0.0 0 0.2" rpy="0 0 1.57"/>
			<geometry>
				<box size="0.1 0.1 0.4"/>
			</geometry>
			<material name="Cyan2">
	       		<color rgba="0 0.7 0.7 1.0"/>
	     	</material>
		</visual>	
	</link>
	
	<!-- * * * Joint Definitions * * * -->
	
	<joint name="articulacion1" type="revolute">
    	<parent link="base"/>
    	<child link="brazo1"/>
    	<origin xyz="0 0.0 0.0" rpy="0 0 0"/>
    	<axis xyz="0 0 1"/>
    	<limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  	</joint>

<joint name="articulacion2" type="revolute">
    	<parent link="brazo1"/>
    	<child link="brazo2"/>
    	<origin xyz="0.05 0.0 0.75" rpy="55 0 0"/>
    	<axis xyz="0 0 1"/>
    	<limit lower="-1.57" upper="1.57" effort="0" velocity="1"/>
  	</joint>

<joint name="articulacion3" type="revolute">
    	<parent link="brazo2"/>
    	<child link="brazo3"/>
    	<origin xyz="0.45 0 0.05" rpy="0 0 1.57"/>
    	<axis xyz="0 1 0"/>
    	<limit lower="-1.57" upper="1.57" effort="0" velocity="1"/>
  	</joint>
	

</robot>


Mientras que los resultados de la simulación se muestran en las siguietnes imágenes:

![image](https://github.com/alfuc55/Lab4/assets/132300202/19ef9588-3b59-4844-93d9-f5d922be0d74)

![image](https://github.com/alfuc55/Lab4/assets/132300202/cb7df69d-ba34-4d3e-ae03-6390cdbfe66a)

![image](https://github.com/alfuc55/Lab4/assets/132300202/8cb05c2a-4037-46e8-8180-a97340bd53e9)




## Plot Juggler 
Con el objetivo de comprobar los resultados del controlador desarrollado e implementado en la práctica anterior, se deberá usar la herramienta plot juggler para graficar las variables y comprobar que el error es muy cercano a cero y asi verificar la precisión del controlador. 
Una vez que plot juggler es instalado en la misma maquina Linux junto con ROS, se procede a levantar el Core de ROS y luego a abrir el simulador turtlesim, después se ejecuta el nodo de ROS programado en Python de la practica anterior. Para que la herramienta de graficación plot juggler pueda leer los datos se conecta a la red de ros como un subscrber y se puede subscribir a todos los nodos disponibles para leer las variables en tiempo real y crear las gráficas.  En cuanto a las variables del sistema que corresponden a velocidad y posición ya están siendo publicadas a la red de ros en el nodo programado en la práctica anterior, por lo que ya es posible graficarlas mediante plot juggler, sin embargo el error no está siendo publicado a la red de ROS y por lo tanto no hay forma de que plot juggler tenga acceso a la variable, por lo que para que sea posible su graficación es necesario crear un tópico de ROS donde se Publique el mensaje y pueda ser leído plotjuggler mediante la red de ROS. 
Para publicar el error en un nuevo tópico de ROS son agregadas las siguientes líneas al código del nodo implementado en la práctica anterior
Incluir el tipo de mensaje FLOAT64 mediante la librería std_msgs.msg
        from std_msgs.msg import Float64	

Después, dentro de la función inicial llamada init crear el publicador, también es necesario darle un valor mayor al buffer para que almacene mas datos y la grafica sea mas extensa
        self.errorPub = rospy.Publisher('/turtle1/error_topic',Float64, queue_size=1000)

Por último, después de la línea que publica el error en la consola: 
        rospy.loginfo(float(error_x))
Agregar la línea: 
            self.errorPub.publish(error_x)
Que publicara el error en x en la red de ROS. 
Una vez que el código ha sido modificado al ejecutarlo ahora se debe estar creando otro topic que manda el error en x de la tortuga.
El ultimo paso es abrir plotjuggler mediante el comando: 
		Rosrun plotjuggler plotjuggler. 
Para la graficación de variables se deberá iniciar el graficador mediante el botón start, esto desplegara la ventana con los tópicos disponibles, aquí se seleccionan los valores que se desean graficar, en nuestro caso el valor mas importante para verificar el funcionamiento del controlador el valor del error, y también la posición en x, theta y la velocidad lineal en x. Se seleccionarán estos tópicos en la ventana y se dará en OK.

![image](https://github.com/alfuc55/Lab4/assets/110939965/5bacb779-4f5a-4812-bd40-091ca54131e1)

Ahora para una mejor presentación de los resultados se dividirá verticalmente la ventana y se arrastraran las variables deseadas para que se grafiquen. 
Ahora al ingresar las coordenadas deseadas para la tortuga plotjuggler deberá graficar en tiempo real las variables asignadas. 

![image](https://github.com/alfuc55/Lab4/assets/110939965/c1eb71a8-116f-42ab-969d-29bb5219664a)


