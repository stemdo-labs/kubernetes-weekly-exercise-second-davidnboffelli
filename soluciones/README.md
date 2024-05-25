# ENUNCIADO Y ANOTACIONES

## Laravel ##

Se usó la imagen de bitnami/laravel y, se propone, en su version 11.0.8<br>
Laravel tiene limites y requests de CPU y memoria.<br>
&nbsp;&nbsp;&nbsp;No se creó un LimitRange, sino que se asignaron directamente en el deployment de laravel<br>
Laravel tiene un readinessProbe y un livenessProbe<br>
Volumen persistente para Laravel<br>
&nbsp;&nbsp;&nbsp;Se usaron volumenes dinámicos. Los PV se crean automáticamente según necesidad.<br>
Ingress para acceder a Laravel desde un navegador<br>

## BBDD ##

Se usó la imagen oficial de la BBDD mysql y, se propone, en su version 8.0<br>
Mysql tiene limites y requests de CPU y memoria.<br>
&nbsp;&nbsp;&nbsp;No se creó un LimitRange, sino que se asignaron directamente en el deployment de mysql<br>
Volumen persistentes para la BBDD<br>
&nbsp;&nbsp;&nbsp;Se usaron volumenes dinámicos. Los PV se crean automáticamente según necesidad.<br>
No es un deployment, sino un statefulset<br>

## phpMyAdmin ##

Se usó la imagen oficial de phpmyadmin y, se propone, en su version 5.2.1<br>
Ingress para acceder a phpmyadmin desde un navegador<br>
Se usó el mismo ingress que laravel<br>
phpmyadmin solicita autenticación para acceder<br>

## Chart ##

En el value correspondiente, además de las variables default de los chart, se pueden configurar las siguientes variables:<br>

namespace: Nombre del namespace común para todos los recursos que despliega el chart<br>

laravelReplicas: Número de réplicas que gestionará el deployment de laravel<br>
laravelTag: Versión de la imagen de bitnami/laravel que se usará<br>
laravelMemorylimit: Límite de memoria que tendrá el deployment de laravel<br>
laravelCPULimit: Límite de CPU que tendrá el deployment de laravel<br>
laravelMemoryRequest: Valor de memoria mínimo que le aseguraremos al deployment de laravel<br>
laravelCPURequest: Valor de CPU mínimo que le aseguraremos al deployment de laravel<br>
laravelPVCStorage: Tamaño del volumen persistente que usará laravel<br>

phpmyadminReplicas: Número de réplicas que gestionará el deployment de phpmyadmin<br>
phpmyadminTag: Versión de la imagen oficial de phpmyadmin que se usará<br>
phpmyadminIsDeployed: Define si se desplegará o no phpmyadmin. Adopta los valores "true" y "false"<br>

mysqlReplicas: Número de réplicas que gestionará el deployment de mysql<br>
mysqlTag: Versión de la imagen oficial de mysql que se usará<br>
mysqlMemorylimit: Límite de memoria que tendrá el deployment de mysql<br>
mysqlCPULimit: Límite de CPU que tendrá el deployment de mysql<br>
mysqlMemoryRequest: Valor de memoria mínimo que le aseguraremos al deployment de mysql<br>
mysqlCPURequest: Valor de CPU mínimo que le aseguraremos al deployment de mysql<br>
mysqlPVCStorage: Tamaño del volumen persistente que usará mysql<br>

dbName: Nombre de la base de datos mysql<br>
dbUser: Nombre de usuario que accederá en la base de datos mysql<br>
dbPass: Password que usará el usuario de la base de datos mysql<br>
dbRootPass: Password del usuario ROOT de la base de datos mysql<br>

ingressClassName: Nombre del ingressClass a usar<br>
ingressHost: Host por el que se accederá a los servicios desplegados desde el navegador<br>
ingressPathLaravel: Path por el que se accederá al servicio de laravel<br>
ingressPathPhpmyadmin: Path por el que se accederá al servicio de phpmyadmin<br>

cronjobPVCStorage: Tamaño del volumen persistente que donde se guardará el backup de la base de datos mysql<br>
cronjobSchedule: Configuración del momento de ejecución del cronjob según documentación de Helm<br>
cronjobBackupName: nombre del archivo sql donde se guardará el backup de la base de datos mysql<br>

##### Valores en el Value por defecto:<br>
namespace: laravel<br>

laravelReplicas: 1<br>
laravelTag: 11.0.8<br>
laravelMemorylimit: "200Mi"<br>
laravelCPULimit: "2"<br>
laravelMemoryRequest: "100Mi" <br>
laravelCPURequest: "0.5"<br>
laravelPVCStorage: 500Mi<br>

phpmyadminReplicas: 1<br>
phpmyadminTag: 5.2.1<br>
phpmyadminIsDeployed: true<br>

mysqlReplicas: 1<br>
mysqlTag: 8.0<br>
mysqlMemorylimit: "800Mi"<br>
mysqlCPULimit: "2"<br>
mysqlMemoryRequest: "500Mi"<br>
mysqlCPURequest: "1"<br>
mysqlPVCStorage: 500Mi<br>

dbName: weeklyDB<br>
dbUser: dnazareno<br>
dbPass: passmuyseguro<br>
dbRootPass: passmuyseguroperoroot<br>

ingressClassName: nginx<br>
ingressHost: k8sweekly.com<br>
ingressPathLaravel: /laravel<br>
ingressPathPhpmyadmin: /phpmyadmin<br>

cronjobPVCStorage: 500Mi<br>
cronjobSchedule: "@daily"<br>
cronjobBackupName: backup-$(date +'%Y%m%d%H%M%S')<br>

Se crearon dos values de ejemplo con un lanzamiento de Laravel (solo laravel y BBDD) y otro con un lanzamiento de Laravel (laravel y BBDD) y phpmyadmin.<br>
Son los archivos "value-laravel.yaml" y "value-laravel-pma.yaml"<br>

## Cronjob DUMP ##

Se incluye un cronjob que se encarga de hacer un backup de la BBDD de Laravel cada 24 horas.<br>
El backup se guarda en un volumen persistente<br>

## Configuración del Schedule según documentación de Helm ##

__________________ minute (0 - 59)<br>
| &nbsp;&nbsp;  _______________ hour (0 - 23)<br>
| &nbsp;&nbsp; | &nbsp;&nbsp;   ____________ day of the month (0 - 23)<br>
| &nbsp;&nbsp; | &nbsp;&nbsp;  |&nbsp;&nbsp;  _________ month (1 - 12)<br>
| &nbsp;&nbsp; | &nbsp;&nbsp;  |&nbsp;&nbsp;  |&nbsp;&nbsp;&nbsp;  ______ day of the week (0 - 6, start con sunday)<br>
| &nbsp;&nbsp; | &nbsp;&nbsp;  |&nbsp;&nbsp;  |&nbsp;&nbsp;&nbsp;  |<br>
| &nbsp;&nbsp; | &nbsp;&nbsp;  |&nbsp;&nbsp;  |&nbsp;&nbsp;&nbsp;  |<br>
\*&nbsp;&nbsp;&nbsp;\*&nbsp;&nbsp;&nbsp;\*&nbsp;&nbsp;&nbsp;\*&nbsp;&nbsp;&nbsp;\*<br>

@yearly or @annually    anualmente el 1 de enero              0 0 1 1 *<br>
@monthly                mensualmente el primer día del mes    0 0 1 * *<br>
@weekly                 semanalmente el domingo a medianoche  0 0 * * 0<br>
@daily                  diariamente a medianoche              0 0 * * *<br>
@hourly                 a cada hora,a los 0 minute            0 * * * *<br>
