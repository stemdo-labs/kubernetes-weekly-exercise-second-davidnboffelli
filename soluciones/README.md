#####################################################################################################################
### ENUNCIADO Y ANOTACIONES ###
#####################################################################################################################
#####################################################################################################################
# Laravel:
#####################################################################################################################
# Se usó la imagen de bitnami/laravel y, se propone, en su version 11.0.8
# Laravel tiene limites y requests de CPU y memoria.
#       No se creó un LimitRange, sino que se asignaron directamente en el deployment de laravel
# Laravel tiene un readinessProbe y un livenessProbe
# Volumen persistente para Laravel
#       Se usaron volumenes dinámicos. Los PV se crean automáticamente según necesidad.
# Ingress para acceder a Laravel desde un navegador

#####################################################################################################################
# BBDD:
#####################################################################################################################
# Se usó la imagen oficial de la BBDD mysql y, se propone, en su version 8.0
# Mysql tiene limites y requests de CPU y memoria.
#       No se creó un LimitRange, sino que se asignaron directamente en el deployment de mysql
# Volumen persistentes para la BBDD
#       Se usaron volumenes dinámicos. Los PV se crean automáticamente según necesidad.
# No es un deployment, sino un statefulset

#####################################################################################################################
# phpmyadmin:
#####################################################################################################################
# Se usó la imagen oficial de phpmyadmin y, se propone, en su version 5.2.1
# Ingress para acceder a phpmyadmin desde un navegador
#       Se usó el mismo ingress que laravel
# phpmyadmin solicita autenticación para acceder

#####################################################################################################################
# chart:
#####################################################################################################################
# En el value correspondiente, además de las variables default de los chart, se pueden configurar las siguientes variables:

# namespace: Nombre del namespace común para todos los recursos que despliega el chart

# laravelReplicas: Número de réplicas que gestionará el deployment de laravel
# laravelTag: Versión de la imagen de bitnami/laravel que se usará
# laravelMemorylimit: Límite de memoria que tendrá el deployment de laravel
# laravelCPULimit: Límite de CPU que tendrá el deployment de laravel
# laravelMemoryRequest: Valor de memoria mínimo que le aseguraremos al deployment de laravel
# laravelCPURequest: Valor de CPU mínimo que le aseguraremos al deployment de laravel
# laravelPVCStorage: Tamaño del volumen persistente que usará laravel

# phpmyadminReplicas: Número de réplicas que gestionará el deployment de phpmyadmin
# phpmyadminTag: Versión de la imagen oficial de phpmyadmin que se usará
# phpmyadminIsDeployed: Define si se desplegará o no phpmyadmin. Adopta los valores "true" y "false"

# mysqlReplicas: Número de réplicas que gestionará el deployment de mysql
# mysqlTag: Versión de la imagen oficial de mysql que se usará
# mysqlMemorylimit: Límite de memoria que tendrá el deployment de mysql
# mysqlCPULimit: Límite de CPU que tendrá el deployment de mysql
# mysqlMemoryRequest: Valor de memoria mínimo que le aseguraremos al deployment de mysql
# mysqlCPURequest: Valor de CPU mínimo que le aseguraremos al deployment de mysql
# mysqlPVCStorage: Tamaño del volumen persistente que usará mysql

# dbName: Nombre de la base de datos mysql
# dbUser: Nombre de usuario que accederá en la base de datos mysql
# dbPass: Password que usará el usuario de la base de datos mysql
# dbRootPass: Password del usuario ROOT de la base de datos mysql

# ingressClassName: Nombre del ingressClass a usar
# ingressHost: Host por el que se accederá a los servicios desplegados desde el navegador
# ingressPathLaravel: Path por el que se accederá al servicio de laravel
# ingressPathPhpmyadmin: Path por el que se accederá al servicio de phpmyadmin

# Valores en el Value por defecto:
# namespace: laravel

# laravelReplicas: 1
# laravelTag: 11.0.8
# laravelMemorylimit: "200Mi"
# laravelCPULimit: "2"
# laravelMemoryRequest: "100Mi" 
# laravelCPURequest: "0.5"
# laravelPVCStorage: 500Mi

# phpmyadminReplicas: 1
# phpmyadminTag: 5.2.1
# phpmyadminIsDeployed: true

# mysqlReplicas: 1
# mysqlTag: 8.0
# mysqlMemorylimit: "800Mi" 
# mysqlCPULimit: "2"
# mysqlMemoryRequest: "500Mi"
# mysqlCPURequest: "1"
# mysqlPVCStorage: 500Mi

# dbName: weeklyDB
# dbUser: dnazareno
# dbPass: passmuyseguro
# dbRootPass: passmuyseguroperoroot

# ingressClassName: nginx
# ingressHost: k8sweekly.com
# ingressPathLaravel: /laravel
# ingressPathPhpmyadmin: /phpmyadmin

# Se crearon dos values de ejemplo con un lanzamiento de Laravel (solo laravel y BBDD) y otro con un lanzamiento de Laravel (laravel y BBDD) y phpmyadmin.
# Son los archivos "value-laravel.yaml" y "value-laravel-pma.yaml"

# Se debe crear un README.md con la información necesaria para desplegar el chart.

# Opcional
# Incluiremos un cronjob que se encargue de hacer un backup de la BBDD de Laravel cada 24 horas.
# El backup se guardará en un volumen persistente
# Se debe poder configurar la hora de ejecución del cronjob desde el values.yaml