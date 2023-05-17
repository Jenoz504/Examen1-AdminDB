# Examen1-AdminDB
Examen1

Como primer paso recomiendo *abrir el diagrama entidad relación* que se encuentra dentro de el repositorio.

# Análisis

Lo primero que analicé fueron estas entidades:
*-Vendedores*
	Fecha en la que les vamos a pagar
	
*-Pedidos* (Los que hacemos con regularidad)

*-Productos* 
	Agregar campo de ventas por semana
	Poner booleano que me sirva para saber si el precio debe ser el 30%
	
*-Clientes totales*

*-Cuentas de créditos*

*-Abonos*

*-Caja*  
	    Cierre y apertura
  
*-Pagos y gastos*   

*-Ingresos*



Como primera parte en el diagrama tenemos el problema de los vendedores. Este lo solucionamos con el conjunto de las primeras 4 tablas;
Proveedores, Vendedor, Productos y Pedidos.

La tabla vendedor guarda los datos personales de la persona que nos vende o promociona los productos, también en el campo "cuenta", se encuentra lo que le debemos actualmente al vendedor, este campo se calcula desde el frontEnd y se registra en la DB. Los pedidos se hacen directamente con el proveedor,
por tanto vamos a dividir los pedidos individualmente en una tabla llamada PEDIDOS. Los pedidos tienen un campo llamado "Compra", el cual guarda un Json con todo lo que compramos, esto se trabaja desde el frontend para poder guardar el costo total de el pedido, y la insersión de los productos nuevos. Por otro lado, recordemos que los pedidos pueden ser al contado o al crédito, por tanto esta tabla contiene el campo de "Fecha de pago", en el cual se marca la fecha en la que pagaremos o en la que realizamos el pago. Se menciona también que hay pedidos que se entregan después, por eso está el campo de entregado, y el de pagado se encuentra para llevar un control y saber cuando un pedido se pagó o si aún se debe (Esto nos servirá más adelante también para llevar el control de caja). 

La tabla productos tiene los datos de los productos en sí. ahí tenemos las existencias que se irán sumando cuando los pedidos son entregados. esto se controla desde el Frontend.

Otro problema que encontramos, es que se quiere saber el precio de los productos, esto para agregarle el 30% al costo o el precio que nos da predefinido el vendedo. Por esta razón se crea la tabla PROVEEDORES, que contiene una llave compuesta con el id del vendedor que nos entrega el producto y el código del producto que este vende, Dos vendedores pueden tener el mismo producto, pero estos lo pueden vender a diferente precio, lo hacemos en una tabla a parte y con una llave compuesta con el fin de cumplir la tercera FN, por esta razón solo se guardan los Key y no el nombre, evitando la redundancia de datos, precio depende de la llave compuesta.


# Caja
El ejercicio nos pide llevar un control de caja por turnos, una apertura y un cierre de la misma. al momento de abrir la caja por lo general hay un capital existente, por eso se registra la cantidadINicial. Inicio contiene la hora de inicio del turno, y final la hora a la que termina el turno. 
Desde el frontEnd, en todo momento se juega con los ingresos, los abonos y los gastos, los cuales modifican constantemente la caja. Por lo que tenemos un campo que se actualiza constantemente llamado "Cantidad actual". El campo cantidadCIerre, tiene por valor el dinero fisico que se encuentra en la caja al momento de cerrrar,por lo que nos daremos cuenta si en ese turno hizo falta dinero o si sobró.

Ingresos tiene como objetivo registrar todo el dinero que entra a parte de las ventas, ya sean bonos o cualquier otro tipo de ingreso, por ello, tiene un campo de descripción, en el cual se puede especificar de qué parte vino ese ingreso. Tomar en cuenta que estos ingresos no incluyen los abono que los clientes nos hacen a las cuentas de créditos que nos deben.

Los gastos de cualquier tipo, se encuentran en la tabla Gastos, incluyendo los pagos de pedidos, por eso está la posibilidad de conectar como llave foránea el idPedido con un pedido que se haya realizado, en caso de que no sea un gasto por pago de pedido, el idcuetna puede quedar vacío y solo puede pasar a describir que tipo de gasto fue, ya sea del hogar, servicio público, lo que sea. Estos los dividimos en el campo "Tipo". Este campo contiene el tipo de gasto, nos servirá para hacer informés de cuanto se gasta en servicios, en el hogar, en producto y otros.

# Segmento 2, Créditos.

 Para el manejo de créditos, siempre hace falta un cliente, es el eslabón principal. Este cliente tiene un campo para sí mismo llamado "Cuenta" que es la suma de todas las cuentas no pagadas que tiene, menos los abonos que se hacen parcialmente a estas cuentas. Si necesitamos ver cuanto nos debe fulano desde el frontend, simplemente se mostrarán todas las cuentas pendientes que estén vinculadas al id del cliente. cuentas que se encuentran en la tabla "Créditos". Necesitamos saber cuales son los clientes que nos pagan puntuales, por eso hay una fecha de pago, que debe respetarse, de lo contrario el sistema lanza alerta de que el cliente se encuentra deudor.  Por el lado de los abonos, abarcan todos los pagos que nos hacen los clientes a las cuentas que deben. 

De esta manera solucionaría yo los problemas presentes en el Mercadito. Saludos!.
