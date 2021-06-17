FROM node:lts-alpine3.12

LABEL AUTHOR="zhaozhanzhan" \
      EMAIL="799316652@qq.com" \
      VERSION=1.0.0

ARG KEY="-----BEGIN RSA PRIVATE KEY-----\nMIIEogIBAAKCAQEAtjLnqHIj/BToYs9sn2HO16/UdRk+uRP/NLjckEhlx9pphuHx\nAaIr6+c8c8P2L2KKYo62AfXfk25ADaQmtBSCcOpRY6YDqdnnXeshoHWvALFHDEUn\nSB4ODBQ8a2ceT/tDNdOnrcM3QW1lI6X1lgBaLHc3KwUSCrD10pgdW4WmLLx5vH09\nPRiBJ+2j/+SD2c/ia1l/CwhFLXkcAAbQX+iORiILd2pnYnnmtbfAZnCseW0bmSx4\nwTQEHzsL79iQorKzwD1N4vcge8bTU0HnzR27Qjp8gdspP62/bpYYSVRBguzPbbHi\nMknHELKAp8Gp2oU1JJq2gdByjgoWG8uYRUTk7QIDAQABAoIBAFNsy5XdbYynJiDY\nSSxRZJxt7iIQBSBJf7lsS9Hk1HXGFDumiOvujZ3eq1d2iur3e9XU8pHDOzQOb1XP\nq8kY+wEoyUuPYG4NUFs4ppUKTa9LfPSpVWs4F8aAHYowFTkaG5Qx7b0qfcp/hKfl\n/aVJVAC4NZKFJ9WtYWGkNQ0LDSO+A1V+k3XPwcGoYh12f7JmSVP9STto7LHZ+XDe\ndOuJ2M4WHPfnlGkzwAypyfAocLliA8nXk3+bacQahzAqsqiScQEL69eqPqHfxxNA\nGY3zrIBmLObRScGYXlGM0BtxqfX6xnhfCQSbAjvqLfNkbQgUY3Ce+OTJig3Q1LNm\n8ulPgyECgYEA2kiNf1OTaqC/eTM7FxsAHSXVLd1nB0UOUdtv5N5HlyuOZpdWgMYH\ngp0AN8wL1YdE/+CbCFMwMMMF75Bn8xusuUFY8lXxTddcgbCcV0KUrmVC2vFWjTl6\nEpdd8iOwaD86ZVtdGudzMsbNEXUGKbB8vLu65ifveo5c00R0HtP14IsCgYEA1a40\nUsr8QOX1jpjbIkdTY9yQAuQtFQDciB9V1GveNx8XIf3dD3w7cLgPuPmDEPEKlLFO\nGcLbaStxAUFThHPNUZ6EbUPO9zAzpM3sXZN/4k9CujpV7DELNF2kYH9NrEqvpX06\nw5Nrk1elD2XmBAzPhWcCz/pPZ5cp7fjjiN1TR2cCgYBgtpai3HfLXY+Ydp9uvFLP\nl0ydtqQIb4tJr20jPW0AyJaeU91KzvLS5xcKoch4gieLK4vlI16NrEr7+sJjz1Y2\n+oIbVQLGF25wM70BWImhv1Hgyd8sUVQJ2rEQMMnfVmxcLh/RZM0AqV6AppXlcMHD\nqXXEtdsANQVhwcmlQwqEwwKBgDvPTwX9my3vMBSB5JRrsoO5MPYeOCAw5YmUe8nB\nh6mWIjt5e27iLyEV5BC6hl0vmu6T0nCTgial6jOXvXDNmSLHY/nRiRTXgR0YJpjq\nuFj4Jo1J1tMNxz0ZGPGJsIwRO6jurIwhr3LGjYUJ2KA5lbyZYMgmICO2xB3AFLAy\ngyTxAoGAKlmiI7e2H5nn3JWhTlUbVOczYBwAymoNxh43ZW+DSh3opucA33jc56SV\nYHgwzYi6R9wYw6YnFuvt2afM3kHrIsOwp/phh33Lw9ml3PCeHlxxp/aA8t9V4sUh\nHc8jUMdN2rjg2qRcCCAhJH8uor0IKcl9cDTR3iglMuJjivkKdlw=\n-----END RSA PRIVATE KEY-----"

ENV DEFAULT_LIST_FILE=crontab_list.sh \
    CUSTOM_LIST_MERGE_TYPE=append \
    COOKIES_LIST=/scripts/logs/cookies.list \
    REPO_URL=git@gitee.com:zhaozhanzhan520/jd_docker.git \
    REPO_BRANCH=master

RUN set -ex \
    && apk update \
    && apk upgrade \
    && apk add --no-cache bash tzdata git moreutils curl jq openssh-client \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && mkdir -p /root/.ssh \
    && echo -e $KEY > /root/.ssh/id_rsa \
    && chmod 600 /root/.ssh/id_rsa \
    && ssh-keyscan gitee.com > /root/.ssh/known_hosts \
    && git clone -b $REPO_BRANCH $REPO_URL /scripts \
    && cd /scripts \
    && mkdir logs \
    && npm config set registry https://registry.npm.taobao.org \
    && npm install \
    && cp /scripts/docker/docker_entrypoint.sh /usr/local/bin \
    && chmod +x /usr/local/bin/docker_entrypoint.sh

WORKDIR /scripts

ENTRYPOINT ["docker_entrypoint.sh"]

CMD [ "crond" ]