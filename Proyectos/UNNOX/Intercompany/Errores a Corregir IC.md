
<mark style="background: #FF5582A6;">A nivel de línea sigue mandando las líneas sin completar con toda la información. (APQ, ADR, tipo paletización, fecha entrega planificada, fecha envio planificada, fecha envio, nº bultos) + Implantación desarrollo V16 Jose</mark>
Falta traducciones al Francés
Mensaje error tras facturar en la empresa B, (falso mensaje de error ya que sí que genera la factura)

RF008, NO se ha generado el albarán de compra ni el de venta en la empresa A. Lo último que se hizo fue crear la factura desde la empresa C. Según el RF008 ya se tendría que haber creado el albarán de compra y venta. 


El consultor no había tenido en cuenta que para que funcione el RF008, también hay que especificar la información correspondiente al lote (me da este error en BC a la hora de generar el albarán de compra de manera manual en la empresa A). Por lo que necesitamos trasladar de manera automática también en el proceso de generar la factura y el RF008, la información correspondiente a los campos que están en la página Lins seguim prod.

En el último commit se han incluido mensajes de debug para copiar/pegar a la IA, para que pueda traquear la información que se está reteniendo con respecto a la lin seguin prod. <mark style="background: #BBFABBA6;">Seguir por aqui</mark>





<mark style="background: #FFF3A3A6;">Evolutivos</mark>

RF008, En Pedido de compra de la empresa A, BC estandard lanza un mensaje azul en la parte superior, se crean facturas automáticas al facturar en la empresa C, ésto no quiero que suceda, no quiero que se creen facturas basura fantasma.

Dirección cliente, en el informe,  Dirección del cliente tiene que tener los datos del cliente final, los datos de dirección del cliente mismos que tiene en dirección envío. RF007 Ojo también con el número de albarán

Estar pendiente de Pedidos ventas, que no se generen pedidos fantasma para CLIENTE PRUEBA, vigilar que no se genere basura al comienzo del proceso.