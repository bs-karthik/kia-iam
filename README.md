### SVD Promethues Metrics

This project can be used to export the SVD Metrics like in a SVD Deployment on Kubernetes

1. Metrics exposed by CN=MONITOR 
2. Replication Stats for each suffix 

Included files runs as a side car container in each SVD Server and exposes the Prometheus endpoint 

#Running the Project 

1. Run the docker build command `docker build -t systemone/svdpromethues .`
2. Update the SVD Deployment to run the systemone/svdpromethues as side car 
3. Update the LDAP_BINDDN and LDAP_BINDPWD a read only user that can search the OU 
4. Update the suffix that need to be search for replication related entries that is ; separated. 
`- name: svdpromethues
        image: systemone/svdpromethues
        imagePullPolicy: IfNotPresent
        env:
          - name: LDAP_BINDDN
            value: "cn=isvaadmin,CN=IBMPOLICIES"
          - name: LDAP_BINDPWD                                   # This will be used in the Logname
            valueFrom:
              secretKeyRef:
                name: isvd
                key:  admin-password
          - name: LDAP_SUFFIX                                   # This will be used in the Logname
            value: "DC=SYSTEMONE.COM;SECAUTHORITY=DEFAULT;CN=IBMPOLICIES"
        ports:
        - containerPort: 8080`
5. Update the SVC to expose the 8080 port 
'- port: 8080
    name: svdpromethues
    protocol: TCP
    targetPort: 8080'
6. Restart the SVD Deployment 
7. Verify the metrics endpoint  

Note: This project only provides a sample and has not been extensively tested. 
