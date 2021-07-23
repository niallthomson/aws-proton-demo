FROM public.ecr.aws/amazonlinux/amazonlinux:2

ARG VERSION="latest"

LABEL version="${VERSION}"

# hadolint ignore=DL3033
RUN yum install -y \
    shadow-utils curl && \
    yum clean all

ENV APPUSER=appuser
ENV APPUID=1000
ENV APPGID=1000

RUN useradd \
    --home "/app" \
    --create-home \
    --user-group \
    --uid "$APPUID" \
    "$APPUSER"

ARG jdk_version=11.0.11.9-1
# In addition to installing the Amazon corretto, we also install
# fontconfig. The folks who manage the docker hub's
# official image library have found that font management
# is a common usecase, and painpoint, and have
# recommended that Java images include font support.
#
# See:
#  https://github.com/docker-library/official-images/blob/master/test/tests/java-uimanager-font/container.java

# The logic and code related to Fingerprint is contributed by @tianon in a Github PR's Conversation
# Comment = https://github.com/docker-library/official-images/pull/7459#issuecomment-592242757
# PR = https://github.com/docker-library/official-images/pull/7459
# hadolint ignore=DL3033,SC2155
RUN set -eux \
    && export GNUPGHOME="$(mktemp -d)" \
    && curl -fL -o corretto.key https://yum.corretto.aws/corretto.key \
    && gpg --batch --import corretto.key \
    && gpg --batch --export --armor '6DC3636DAE534049C8B94623A122542AB04F24E3' > corretto.key \
    && rpm --import corretto.key \
    && rm -r "$GNUPGHOME" corretto.key \
    && curl -fL -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo \
    && grep -q '^gpgcheck=1' /etc/yum.repos.d/corretto.repo \
    && yum install -y java-11-amazon-corretto-devel-$jdk_version \
    && (find /usr/lib/jvm/java-11-amazon-corretto -name src.zip -delete || true) \
    && yum install -y fontconfig \
    && yum clean all

ENV LANG C.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto

WORKDIR /app
USER appuser

COPY ./pom.xml ./mvnw /app/
COPY ./.mvn/ /app/.mvn

RUN ./mvnw dependency:go-offline -B

COPY ./src ./src

RUN ./mvnw -DskipTests --batch-mode --no-transfer-progress package

EXPOSE 8080

CMD ["java", "$JAVA_OPTS", "-jar", "/app/target/app.jar"]