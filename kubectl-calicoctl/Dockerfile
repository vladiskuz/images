FROM quay.io/vladiskuz/toolbox:latest as kctl

RUN apk update && apk add curl

# Inject custom root certificate authorities if needed
# Docker does not have a good conditional copy statement and requires that a source file exists
# to complete the copy function without error.  Therefore the README.md file will be copied to
# the image every time even if there are no .crt files.
COPY ./certs/* /usr/local/share/ca-certificates/
RUN update-ca-certificates

RUN curl -L "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" \
      -o /kubectl
RUN chmod +x /kubectl


FROM quay.io/vladiskuz/toolbox:latest as calicoctl

RUN apk update && apk add curl

# Inject custom root certificate authorities if needed
# Docker does not have a good conditional copy statement and requires that a source file exists
# to complete the copy function without error.  Therefore the README.md file will be copied to
# the image every time even if there are no .crt files.
COPY ./certs/* /usr/local/share/ca-certificates/
RUN update-ca-certificates

RUN curl -L  "https://github.com/projectcalico/calicoctl/releases/download/v3.18.1/calicoctl" \
      -o /calicoctl
RUN chmod +x /calicoctl


FROM quay.io/vladiskuz/toolbox:latest
COPY --from=kctl /kubectl /usr/local/bin/kubectl
COPY --from=calicoctl /calicoctl /usr/local/bin/calicoctl
CMD ["/usr/local/bin/config-function"]
