FROM mojodna/cedar-14-stack

ENV DEBIAN_FRONTEND noninteractive

RUN \
  apt-get upgrade -y

ADD ./proj.tar.gz /tmp
ADD ./proj-datumgrid.tar.gz /tmp/proj-4.8.0/nad/
RUN \
  cd /tmp/proj-* && \
  ./configure --without-mutex --prefix=/app/vendor/proj && \
  make install && \
  cd /app/vendor/proj && \
  tar zcf /tmp/proj-cedar-14.tar.gz .

ADD ./gdal.tar.gz /tmp
RUN \
  cd /tmp/gdal-* && \
  ./configure --prefix=/app/vendor/gdal \
              --with-static-proj4=/app/vendor/proj \
              --with-pcraster=no \
              --with-jasper=no \
              --with-grib=no \
              --with-vfk=no \
              --with-hide-internal-symbols \
              --with-java=/usr/lib/jvm/java-7-openjdk-amd64 \
              --with-jvm-lib=/usr/lib/jvm/java-7-openjdk-amd64/jre/lib/amd64/server \
              --with-mdb=yes \
              --with-jvm-lib-add-rpath=yes && \
  make install && \
  cd /app/vendor/gdal && \
  tar zcf /tmp/gdal-cedar-14.tar.gz .

RUN \
  cp -a /libs/. /usr/lib/jvm/java-7-openjdk-amd64/jre/lib/ext/
