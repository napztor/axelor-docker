FROM axelor/app-builder as builder

RUN mkdir -p /app/src
WORKDIR /app/src

RUN \
  set -ex && \
  git clone git@github.com:axelor/open-suite-webapp.git open-suite-webapp && \
  sed -e 's|git@github.com:|https://github.com/|' -i open-suite-webapp/.gitmodules && \
  cd open-suite-webapp && \
  git checkout master && \
  git submodule sync && \
  git submodule init && \
  git submodule update && \
  git submodule foreach git checkout master && \
  git submodule foreach git pull origin master && \
  sed -e 's|^application.theme.*|application.theme = modern|g' -i src/main/resources/application.properties && \
  ./gradlew --no-daemon -x test npm-build build

FROM axelor/app-base-image
LABEL maintainer="Axelor <support@axelor.com>"

COPY --from=builder /app/src/open-suite-webapp/build/libs/open-suite-webapp-*.war $CATALINA_BASE/webapps/ROOT.war

CMD ["start"]
