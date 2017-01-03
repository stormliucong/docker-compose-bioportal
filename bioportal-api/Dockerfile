FROM ruby:2.2.5
EXPOSE 8080:80

RUN mkdir /srv/ncbo/

RUN apt-get update
RUN apt-get -y install nginx-full supervisor
COPY nginx.conf /etc/nginx.conf

RUN cd /srv/ncbo && git clone https://github.com/sifrproject/ontologies_api.git && git clone https://github.com/sifrproject/ncbo_cron.git

RUN cd /srv/ncbo/ontologies_api/ && bundle install

RUN cd /srv/ncbo/ncbo_cron/ && bundle install 

RUN gem install rails

COPY ncbo_cron_init /srv/ncbo/

COPY config_cron.rb /srv/ncbo/ncbo_cron/config/config.rb
COPY config_api.rb /srv/ncbo/ontologies_api/config/environments/production.rb
COPY unicorn /srv/ncbo/



WORKDIR /srv/ncbo/

VOLUME ["/srv"]

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Define default command.
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]