1. Crie Sua imagem (Dockerfile) 
2. Crie um arquivo indicando os parametros da imagem e demais configurações. Salve como nome_do_app.adoc

    Ex:

    $ git clone https://github.com/redhat-helloworld-msa/ola
    $ cd ola/
    $ oc new-build --binary --name=ola -l app=ola
    $ mvn package; oc start-build ola --from-dir=. --follow
    $ oc new-app ola -l app=ola,hystrix.enabled=true
    $ oc expose service ola
    ----

    ##### Enable Jolokia and Readiness probe

    ----
    $ oc env dc/ola AB_ENABLED=jolokia; oc patch dc/ola -p '{"spec":{"template":{"spec":{"containers":[{"name":"ola","ports":[{"containerPort": 8778,"name":"jolokia"}]}]}}}}'
    $ oc set probe dc/ola --readiness --get-url=http://:8080/api/health
    ----

    #### Test the service endpoint

    ----
    curl http://ola-helloworld-msa.`minishift ip`.nip.io/api/ola
    ----
    