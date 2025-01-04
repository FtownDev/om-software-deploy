FROM quay.io/keycloak/keycloak:25.0 as builder

# Enable health and metrics support
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_HTTP_ENABLED=true
ENV KC_PROXY_HEADERS=forwarded
ENV KC_LEGACY_OBSERVABILITY_INTERFACE=true

ENV KC_FEATURES="authorization,token-exchange,docker,impersonation,scripts,web-authn,client-policies,dynamic-scopes"

# Configure a database vendor
ENV KC_DB=postgres
ENV KC_TRANSACTION_XA_ENABLED=false

WORKDIR /opt/keycloak
# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:25.0.5
COPY --from=builder /opt/keycloak/ /opt/keycloak/

ENTRYPOINT ["/opt/keycloak/bin/kc.sh"]