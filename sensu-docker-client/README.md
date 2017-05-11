Building and running the thing with a N number

export N="101"; cp -f Dockerfile.tmpl Dockerfile; sed -i "s/CLIENTNUM/$N/g" Dockerfile; docker build -t "sensu-client$N" .; docker run -d --name sensu$N sensu-client$N; docker ps

or building multiples

for N in {01..05}; do cp -f Dockerfile.tmpl Dockerfile; sed -i "s/CLIENTNUM/$N/g" Dockerfile; docker build -t "sensu-client$N" .; docker run -d --name sensu$N sensu-client$N; docker ps; done
