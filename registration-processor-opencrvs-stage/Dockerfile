FROM openjdk:11-jre

# can be passed during Docker build as build time environment for github branch to pickup configuration from | spring profiles active and config server URL
ARG spring_config_label
ARG active_profile
ARG spring_config_url

# environment variable to pass active profile such as DEV, QA etc at docker runtime | config url to pickup configuration from, at docker runtime | github branch to pickup configuration from, at docker runtime
ENV active_profile_env=${active_profile}
ENV spring_config_url_env=${spring_config_url}
ENV spring_config_label_env=${spring_config_label}

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user=mosip
ARG container_user_group=mosip
ARG container_user_uid=1001
ARG container_user_gid=1001

# install packages, create user and change permissions of file inside working dir
RUN apt-get -y update \
&& apt-get install -y unzip \
&& groupadd -g ${container_user_gid} ${container_user_group} \
&& useradd -u ${container_user_uid} -g ${container_user_group} -s /bin/sh -m ${container_user} \
&& chown -R ${container_user}:${container_user} /home/${container_user}

# set working directory for the user
WORKDIR /home/${container_user}

EXPOSE 8045

ADD --chown=${container_user}:${container_user} ./target/registration-processor-opencrvs-stage-*-spring-boot.jar registration-processor-opencrvs-stage.jar

# select container user for all tasks
USER ${container_user_uid}:${container_user_gid}

CMD java -Dspring.cloud.config.label="${spring_config_label_env}" -Dspring.profiles.active="${active_profile_env}" -Dspring.cloud.config.uri="${spring_config_url_env}" -jar registration-processor-opencrvs-stage.jar
