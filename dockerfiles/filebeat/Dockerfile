ARG VERSION=7.0.1

FROM elastic/filebeat:${VERSION}

USER root

RUN \
    chgrp -R 0 /usr/share/filebeat/ /var/log && \
    chmod -R g=u /usr/share/filebeat/ /var/log

USER filebeat

CMD ["-e","-strict.perms=false"]
