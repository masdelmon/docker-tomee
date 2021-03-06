FROM openjdk:8-jre

ENV PATH /usr/local/tomee/bin:$PATH
RUN mkdir -p /usr/local/tomee

WORKDIR /usr/local/tomee

# curl -fsSL 'https://www.apache.org/dist/tomee/KEYS' | awk -F ' = ' '$1 ~ /^ +Key fingerprint$/ { gsub(" ", "", $2); print $2 }' | sort -u
ENV GPG_KEYS \
    223D3A74B068ECA354DC385CE126833F9CF64915 \
    7A2744A8A9AAF063C23EB7868EBE7DBE8D050EEF \
    82D8419BA697F0E7FB85916EE91287822FDB81B1 \
    9056B710F1E332780DE7AF34CBAEBE39A46C4CA1 \
    A57DAF81C1B69921F4BA8723A8DE0A4DB863A7C1 \
    B7574789F5018690043E6DD9C212662E12F3E1DD \
    B8B301E6105DF628076BD92C5483E55897ABD9B9 \
    DBCCD103B8B24F86FFAAB025C8BB472CD297D428 \
    F067B8140F5DD80E1D3B5D92318242FE9A0B1183 \
    FAA603D58B1BA4EDF65896D0ED340E0E6D545F97 \
    C92604B0DEC5C62CFF5801E73D4683C24EDC64D1

RUN set -xe \
    && for key in $GPG_KEYS; do \
		gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
        gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
        gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
    done

RUN set -x \
	&& curl -fSL https://dist.apache.org/repos/dist/release/tomee/tomee-8.0.0-M2/apache-tomee-8.0.0-M2-webprofile.tar.gz.asc -o tomee.tar.gz.asc \
	&& curl -fSL https://dist.apache.org/repos/dist/release/tomee/tomee-8.0.0-M2/apache-tomee-8.0.0-M2-webprofile.tar.gz -o tomee.tar.gz \
	&& gpg --batch --verify tomee.tar.gz.asc tomee.tar.gz \
	&& tar -zxf tomee.tar.gz \
	&& mv apache-tomee-webprofile-8.0.0-M2/* /usr/local/tomee \
	&& rm -Rf apache-tomee-webprofile-8.0.0-M2 \
	&& rm bin/*.bat \
	&& rm tomee.tar.gz*


EXPOSE 8080
CMD ["catalina.sh", "run"]
