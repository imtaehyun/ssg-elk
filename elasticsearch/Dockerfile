FROM elasticsearch:1.7.3

RUN /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head
RUN /usr/share/elasticsearch/bin/plugin install elasticsearch/marvel/latest

CMD ["elasticsearch", "--network.host", "0.0.0.0"]
