FROM centos:latest
RUN yum install rsyslog -y; mkdir /var/log/nginx
ADD ./rsyslog.conf /etc/rsyslog.conf
CMD ["/usr/sbin/rsyslogd", "-n", "-f", "/etc/rsyslog.conf"]
