FROM amazoncorretto:8

LABEL  maintainer "Akria Nitta <akira.nitta+docker@naviplus.co.jp>"

RUN yum install -y \
	git \
	bzip2 \
	make \
	autoconf \
	curl \
	wget \
	xz \
	tar.x86_64 \
	&& rm -rf /var/cache/yum

ENV JRUBY_VERSION 9.2.7.0
ENV JRUBY_SHA256 da7c1a5ce90015c0bafd4bca0352294e08fe1c9ec049ac51e82fe57ed50e1348
ENV RUBYGEMS_VERSION 3.0.2
ENV BUNDLER_VERSION 2.0.1
ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
	BUNDLE_BIN="$GEM_HOME/bin" \
	BUNDLE_SILENCE_ROOT_WARNING=1 \
	BUNDLE_APP_CONFIG="$GEM_HOME" \
	BUNDLE_GEMFILE='/app/Gemfile'

ENV PATH $BUNDLE_BIN:$PATH
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
	&& chmod 777 "$GEM_HOME" "$BUNDLE_BIN"
ENV PATH /opt/jruby/bin:$PATH
RUN mkdir /opt/jruby \
	# JRuby
	&& curl -fSL https://s3.amazonaws.com/jruby.org/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz -o /tmp/jruby.tar.gz \
	&& echo "$JRUBY_SHA256 /tmp/jruby.tar.gz" | sha256sum -c - \
	&& tar -zx --strip-components=1 -f /tmp/jruby.tar.gz -C /opt/jruby \
	&& rm /tmp/jruby.tar.gz \
	&& update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1 \
	&& gem update --system "$RUBYGEMS_VERSION" \
	&& gem install bundler --version "$BUNDLER_VERSION" --force \
	&& rm -r /root/.gem/

# skip installing gem documentation
RUN mkdir -p /opt/jruby/etc \
	&& { \
	echo 'install: --no-document'; \
	echo 'update: --no-document'; \
	} >> /opt/jruby/etc/gemrc
