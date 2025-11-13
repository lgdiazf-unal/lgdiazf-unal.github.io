FROM ruby:3.3.10

RUN mkdir -p /app && chown 1000:1000 -R /app


# Establece el usuario con UID 1000
USER 1000

# Configura Bundler para usar un directorio en el directorio de trabajo
#RUN bundle config --global path ~/.gem/ruby
#RUN bundle config --global cache_path ~/.gem/ruby/cache
WORKDIR /app

COPY ./Gemfile  .

RUN bundle






