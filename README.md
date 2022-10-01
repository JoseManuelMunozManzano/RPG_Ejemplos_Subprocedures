# PRUEBAS RPGLE USANDO ILE PROCEDURES

Se muestran ejemplos de uso de procedimientos.
Para realizar esta prueba se han creado los siguientes fuentes en estos contenedores.

## QDDSSRC

Se ha creado un fichero PF llamado ordmst.pf que contiene datos de posibles órdenes de productos
Para informar datos se pueden ejecutar las siguientes consultas SQL:

```
  INSERT INTO ORDMST VALUES('101', 123456, 'IPhone 14', 1, '2022-10-01');
  INSERT INTO ORDMST VALUES('102', 112312, 'Certabo Chess', 3, '2022-08-03');
  INSERT INTO ORDMST VALUES('103', 198652, 'Rayban Glasses', 7, '2021-12-31');
  INSERT INTO ORDMST VALUES('104', 100149, 'TV Samsung', 1, '2022-08-11');
```

## QDSPSRC

Se ha creado la pantalla ordprfm.dspf para visualizar datos.
Si no se encuentra el valor informado (101, 102, 103 o 104) aparece un mensaje de error. Si se informa aparecerá el descriptivo.

## QRPGLESRC

Se han creado tres ejemplos:

1. Procedure interno en programa
   Es el fuente ordpri01.rpgle.
   Se compila con 14. Notar que se indica dftactgrp a \*no para que se compile como ILE.
   Para ejecutar:

   ```
   CALL ORDPRI01
   ```

2. Procedure en otro programa
   Tenemos dos programas, un programa llamador, ordprc1.rpgle y el programa llamado, donde está el procedure, ordprc2.rpgle.
   Ambos programas se compilan con 14. Notar que se indica dftactgrp a *no para que se compile como ILE. Además, el programa llamador tiene grupo de activación *new y el llamado \*caller, para que se someta al llamador.
   Para ejecutar:

   ```
   CALL ORDPRC1
   ```

3. Utilización de Service Program
   Se ha creado el service program ordsrv.rpgle.
   Notar el nomain indicando que no sigue el ciclo lógico de programa. También indicar que en el procedure se indica export.
   Otra cosa importante es que el fichero está declarado dentro del procedure, algo que antes de la versión 6.1 no era posible. Para funcionar con un fichero en un procedure, indicar que hay que declarar una DS donde se carga la información leida.
   Se compila con los siguientes dos comandos:

   ```
   CRTRPGMOD MODULE(MyLib/ORDSRV) SRCFILE(MyLib/QRPGLESRC)
   CRTSRVPGM SRVPGM(MyLib/ORDSRV) EXPORT(*ALL)
   ```

   Se crea también el programa que hace uso del service program, esto es, ordsrvc1.rpgle.
   Indicar que en este caso, como se compila con CRTRPGMOD, por defecto ya es ILE, y no se indica en el programa dftactgrp.
   Se compila con los siguientes dos comandos (el segundo comando crea realmente el PGM)

   ```
   CRTRPGMOD MODULE(MyLib/ORDSRVC1) DBGVIEW(*SOURCE) SRCFILE(MyLib/QRPGLESRC)
   CRTPGM PGM(MyLib/ORDSRVC1) BNDSRVPGM(ORDSRV) ACTGRP(*NEW)
   ```

   Para ejecutar:

   ```
   CALL ORDSRVC1
   ```
