# This docker is based on ubuntu 16.04
# The objective is to run sen2cor in this docker
FROM ubuntu:xenial

LABEL maintainer "Lufuno Vhengani <lvhengani@csir.co.za>, Alando Ballantyne <alando@radiant.earth>"
ENV DEBIAN_FRONTEND noninteractive

ARG SEN2COR_VERSION='02.05.05'
ARG SEN2COR_DIRECTORY='2.5.5' 
ARG GOSU_VERSION='1.10'
ARG SEN2COR="Sen2Cor-${SEN2COR_VERSION}-Linux64.run"
RUN apt-get update && apt-get install -y \
        unzip \
        curl \
        file \
        wget && \
    rm -rf /var/lib/apt/lists/* && \
    wget "http://step.esa.int/thirdparties/sen2cor/${SEN2COR_DIRECTORY}/${SEN2COR}" && \
    bash ${SEN2COR} && \
    rm ${SEN2COR} && \
    cp -p /Sen2Cor-${SEN2COR_VERSION}-Linux64/lib/python2.7/site-packages/sen2cor/cfg/L2A_GIPP.xml /root/sen2cor/2.5/cfg/L2A_GIPP_without_dem.xml && \
    sed 's:>NONE<:>dem<:' /root/sen2cor/2.5/cfg/L2A_GIPP_without_dem.xml >/root/sen2cor/2.5/cfg/L2A_GIPP_with_dem.xml && \
    curl -o /usr/local/bin/gosu -sSL "https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64" && \
    chmod +x /usr/local/bin/gosu && \
    # use all available cores. note this could cause memory issues -alando
    # (default config, no '-d' arg):
    sed -i.bak 's/<Nr_Processes>1/<Nr_Processes>AUTO/' /root/sen2cor/2.5/cfg/L2A_GIPP.xml && \
    # ('-d' arg configuration):
    sed -i.bak 's/<Nr_Processes>1/<Nr_Processes>AUTO/' /root/sen2cor/2.5/cfg/L2A_GIPP_with_dem.xml
    # same config changes for operation mode 
    # (default config)
    #sed -i.bak 's/<Operation_Mode>PDGS/<Operation_Mode>TOOLBOX/' /root/sen2cor/2.5/cfg/L2A_GIPP.xml && \
    # ('-d' arg config)
    #sed -i.bak 's/<Operation_Mode>PDGS/<Operation_Mode>TOOLBOX/' /root/sen2cor/2.5/cfg/L2A_GIPP_with_dem.xml

ADD docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh
