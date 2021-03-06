# X-Arch Orthanc Plugin Image
# Derek Merck, Fall 2018

ARG DOCKER_ARCH="amd64"
# ARG DOCKER_ARCH="arm32v7"
# ARG DOCKER_ARCH="arm64v8"

FROM orthanc-$DOCKER_ARCH
LABEL description="X-Arch Orthanc, free cross-platform DICOM server (plugins)"

RUN apt -y clean && apt -y update
RUN DEBIAN_FRONTEND=noninteractive apt -y install --no-install-recommends \
    libgdcm2-dev \
    libjpeg-dev \
    postgresql-server-dev-all \
    libtiff-dev \
    libopenslide-dev \
  && apt -y clean \
  && rm -rf /var/lib/apt/lists/*

# libopenjpeg-dev doesn't exist for Debian Stretch and only exists in sid for amd64 and arm64 --
# Manual forward porting is a bad habit, but probably ok for a Docker image where nothing will be updated
RUN curl -O http://ftp.us.debian.org/debian/pool/main/o/openjpeg/libopenjpeg5_1.5.2-3_`dpkg --print-architecture`.deb \
  && curl -O http://ftp.us.debian.org/debian/pool/main/o/openjpeg/libopenjpeg-dev_1.5.2-3_`dpkg --print-architecture`.deb \
  && dpkg --install libopenjpeg5_1.5.2-3_`dpkg --print-architecture`.deb libopenjpeg-dev_1.5.2-3_`dpkg --print-architecture`.deb \
  && rm libopenjpeg*.deb

ADD ./build-postgresql.sh /root/build-postgresql.sh
RUN bash /root/build-postgresql.sh "default"

ADD ./build-webviewer.sh /root/build-webviewer.sh
RUN bash /root/build-webviewer.sh "default"

ADD ./build-dicomweb.sh /root/build-dicomweb.sh
RUN bash /root/build-dicomweb.sh "default"

ADD ./build-wsi.sh /root/build-wsi.sh
RUN bash /root/build-wsi.sh "default"
