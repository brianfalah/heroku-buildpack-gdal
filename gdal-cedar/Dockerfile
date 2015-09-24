FROM mojodna/cedar-stack

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
  tar zcf /tmp/proj-cedar.tar.gz .

ADD ./mdb-sqlite.tar.bz2 /tmp
RUN \
  mkdir /app/vendor/libs
  cp /tmp/mdb-sqlite-1.0.2/lib/{jackcess-1.1.14.jar,commons-lang-2.4.jar,commons-logging-1.1.1.jar} /app/vendor/libs/
  EXPORT CLASSPATH=/app/vendor/libs

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
              --with-java=yes \
              --with-mdb=yes && \
  make install && \
  cd /app/vendor/gdal && \
  tar zcf /tmp/gdal-cedar.tar.gz .
