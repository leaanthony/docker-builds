FROM alpine:latest

ARG VIPS_VERSION=8.10.0
ARG VIPS_URL=https://github.com/libvips/libvips/releases/download

RUN apk update && apk upgrade

# basic packages libvips likes
RUN apk add \
	build-base \
	autoconf \
	automake \
	libtool \
	bc \
	zlib-dev \
	expat-dev \
	jpeg-dev \
	tiff-dev \
	glib-dev \
	libjpeg-turbo-dev \
	libexif-dev \
	lcms2-dev \
	fftw-dev \
	giflib-dev \
	libpng-dev \
	libwebp-dev \
	orc-dev \
	libgsf-dev 

# add these if you like for text rendering, PDF rendering, SVG rendering, 
# but they will pull in loads of other stuff
RUN apk add \
	gdk-pixbuf-dev \
	poppler-dev \
	librsvg-dev 

# there are other optional deps you can add for openslide / openexr /
# imagmagick support / Matlab support etc etc

RUN wget -O- ${VIPS_URL}/v${VIPS_VERSION}/vips-${VIPS_VERSION}.tar.gz \
	| tar xzC /tmp
RUN cd /tmp/vips-${VIPS_VERSION} \
	&& ./configure --prefix=/usr --disable-static --disable-debug \
	&& make V=0 \
	&& make install 

RUN apk add \
	python3-dev \
	py3-pip

# and now pyvips can go on ... bizarrely, we must install cffi explicitly
# (even though pyvips lists it as a dependency), and we must install in two
# phases
# some kind of pip bug?
RUN pip3 install cffi 
RUN pip3 install pyvips

WORKDIR /data
