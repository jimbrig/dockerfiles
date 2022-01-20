FROM ubuntu:16.04 AS SQLSERVER
LABEL maintainer="Jimmy Briggs"

# apt-get and system utilities
RUN apt-get update && apt-get install -y \
	curl apt-transport-https debconf-utils \
    && rm -rf /var/lib/apt/lists/*

# adding custom MS repository
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

# install SQL Server drivers and tools
RUN apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
RUN /bin/bash -c "source ~/.bashrc"

RUN apt-get -y update \
    && apt-get -y upgrade \
    && apt-get -y install mssql-server-extensibility

RUN apt-get -y install locales
RUN locale-gen en_US.UTF-8
RUN update-locale LANG=en_US.UTF-8

FROM SQLSERVER AS RLANG

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y update \
    && apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
RUN apt-get -y update

# Install R runtime
RUN apt-get -y install r-base-core

# add R to launchpadd
RUN echo '\n[Service]\nEnvironment="R_HOME=/usr/lib/R"' >> /etc/systemd/system/mssql-launchpadd.service.d/override.conf

# libR.so
RUN echo '/usr/lib/R/library' >> /etc/ld.so.conf.d/custom-r.conf
RUN ldconfig

RUN /opt/mssql/bin/mssql-conf set extensibility.datadirectories /usr/lib/R
RUN systemctl restart mssql-launchpadd

RUN ${R_HOME}/bin/R -e 'install.packages("Rcpp", lib = "${R_HOME}/library")'

CMD /bin/bash 

